# Taking Photos

> https://developer.android.google.cn/training/camera/photobasics.html﻿

This lesson teaches how to capture a photo by delegating the work to another camera app on the device. (If you'd rather build your own camera functionality, see Controlling the Camera.)

> 本课将告诉您如何通过将工作委派给设备上的其他相机应用来捕获照片。(如果您更愿意构建自己的相机功能，请参阅控制相机。)

Suppose you are implementing a crowd-sourced weather service that makes a global weather map by blending together pictures of the sky taken by devices running your client app. Integrating photos is only a small part of your application. You want to take photos with minimal fuss, not reinvent the camera. Happily, most Android-powered devices already have at least one camera application installed. In this lesson, you learn how to make it take a picture for you.

> 假设您正在实施一个众包天气服务，通过将运行您的客户端应用的设备所拍摄的天空图片混合在一起来制作全球天气图.集成照片只是应用程序的一小部分。你想用最少的手段拍照，而不是重新发明相机。令人高兴的是，大多数基于Android的设备已经安装了至少一个摄像头应用程序。在本课中，您将学习如何为您拍摄照片.

#### Request the camera feature(请求相机功能)

If an essential function of your application is taking pictures, then restrict its visibility on Google Play to devices that have a camera. To advertise that your application depends on having a camera, put a \<uses-feature\> tag in your manifest file:

> 如果您的应用程序的基本功能正在拍摄照片，请将其在Google Play上的可见度限制为拥有相机的设备。要宣传您的应用程序依赖于使用相机，请在清单文件中放入一个\<uses-feature\>标签：

```
<manifest ... >
    <uses-feature android:name="android.hardware.camera"
                  android:required="true" />
    ...
</manifest>
```

If your application uses, but does not require a camera in order to function, instead set `android:required` to `false`. In doing so, Google Play will allow devices without a camera to download your application. It's then your responsibility to check for the availability of the camera at runtime by calling `hasSystemFeature(PackageManager.FEATURE_CAMERA)`. If a camera is not available, you should then disable your camera features.

> 如果您的应用程序使用，但不需要相机才能正常工作，请将android：required设置为false。这样做，Google Play将允许没有摄像头的设备下载您的应用程序。然后，您有责任通过调用hasSystemFeature（PackageManager.FEATURE_CAMERA）来检查运行时相机的可用性。如果相机不可用，则应该禁用相机功能.

#### Take a photo with a camera app(用相机应用拍摄照片)

The Android way of delegating actions to other applications is to invoke an Intent that describes what you want done. This process involves three pieces: The Intent itself, a call to start the external Activity, and some code to handle the image data when focus returns to your activity.

> 将操作委派给其他应用程序的Android方法是调用描述你想要完成的Intent。该过程涉及三个部分：Intent本身，用于启动外部活动的调用，以及在焦点返回到活动时处理图像数据的一些代码

Here's a function that invokes an intent to capture a photo.

> 这是一个调用意图捕获照片的函数。

```
static final int REQUEST_IMAGE_CAPTURE = 1;

private void dispatchTakePictureIntent() {
    Intent takePictureIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
    if (takePictureIntent.resolveActivity(getPackageManager()) != null) {
        startActivityForResult(takePictureIntent, REQUEST_IMAGE_CAPTURE);
    }
}
```

Notice that the startActivityForResult() method is protected by a condition that calls resolveActivity(), which returns the first activity component that can handle the intent. Performing this check is important because if you call startActivityForResult() using an intent that no app can handle, your app will crash. So as long as the result is not null, it's safe to use the intent.

> 请注意，startActivityForResult（）方法受调用resolveActivity（）的条件保护，该条件返回可处理意图的第一个活动组件。执行此检查很重要，因为如果您使用无应用程序可以处理的意图调用startActivityForResult（），则您的应用程序将崩溃。所以只要结果不为空，就可以安全使用意图.

#### Get the thumbnail(获取缩略图)

If the simple feat of taking a photo is not the culmination of your app's ambition, then you probably want to get the image back from the camera application and do something with it.

> 如果拍摄照片的简单技巧不是应用程序雄心壮志的顶点，那么您可能希望从相机应用程序中获取图像并对其进行处理。

The Android Camera application encodes the photo in the return Intent delivered to onActivityResult() as a small Bitmap in the extras, under the key "data". The following code retrieves this image and displays it in an ImageView.

> Android Camera应用程序将照片放入返回的Intent中，并将其作为附加内容中的小数位图传送到onActivityResult（）中的“数据”关键字下。以下代码检索此图像并将其显示在ImageView中。

```
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    if (requestCode == REQUEST_IMAGE_CAPTURE && resultCode == RESULT_OK) {
        Bundle extras = data.getExtras();
        Bitmap imageBitmap = (Bitmap) extras.get("data");
        mImageView.setImageBitmap(imageBitmap);
    }
}
```

> Note: This thumbnail image from "data" might be good for an icon, but not a lot more. Dealing with a full-sized image takes a bit more work.

> 注意：这个来自“数据”的缩略图可能对图标有好处，但不是更多。处理全尺寸图像需要更多工作.

#### Save the full-size photo(保存全尺寸的照片)

The Android Camera application saves a full-size photo if you give it a file to save into. You must provide a fully qualified file name where the camera app should save the photo.

> 如果您为其保存文件，Android Camera应用程序会保存一张全尺寸的照片.您必须提供相机应用程序应该保存照片的完全限定文件名。

Generally, any photos that the user captures with the device camera should be saved on the device in the public external storage so they are accessible by all apps. The proper directory for shared photos is provided by getExternalStoragePublicDirectory(), with the DIRECTORY_PICTURES argument. Because the directory provided by this method is shared among all apps, reading and writing to it requires the READ_EXTERNAL_STORAGE and WRITE_EXTERNAL_STORAGE permissions, respectively. The write permission implicitly allows reading, so if you need to write to the external storage then you need to request only one permission:

> 通常，用户使用设备摄像头拍摄的任何照片都应该保存在公共外部存储设备中，以便所有应用均可访问。共享照片的正确目录由getExternalStoragePublicDirectory（）提供，并带有DIRECTORY_PICTURES参数.由于此方法提供的目录在所有应用程序之间共享，因此读取和写入目录需要分别具有READ_EXTERNAL_STORAGE和WRITE_EXTERNAL_STORAGE权限。写入权限隐式允许读取，因此如果您需要写入外部存储器，则只需要请求一个权限：

```
<manifest ...>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    ...
</manifest>
```

However, if you'd like the photos to remain private to your app only, you can instead use the directory provided by getExternalFilesDir(). On Android 4.3 and lower, writing to this directory also requires the WRITE_EXTERNAL_STORAGE permission. Beginning with Android 4.4, the permission is no longer required because the directory is not accessible by other apps, so you can declare the permission should be requested only on the lower versions of Android by adding the maxSdkVersion attribute:

>但是，如果您希望照片仅保留在您的应用中，则可以使用getExternalFilesDir（）提供的目录.在Android 4.3及更低版本上，写入此目录还需要WRITE_EXTERNAL_STORAGE权限。从Android 4.4开始，不再需要该权限，因为该目录不能被其他应用程序访问，所以你可以通过添加maxSdkVersion属性声明应该只在较低版本的Android上请求权限：

```
<manifest ...>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"
                     android:maxSdkVersion="18" />
    ...
</manifest>
```

> Note: Files you save in the directories provided by getExternalFilesDir() or getFilesDir() are deleted when the user uninstalls your app.

> 注意：当用户卸载您的应用程序时，您保存在由getExternalFilesDir（）或getFilesDir（）提供的目录中的文件将被删除。

Once you decide the directory for the file, you need to create a collision-resistant file name. You may wish also to save the path in a member variable for later use. Here's an example solution in a method that returns a unique file name for a new photo using a date-time stamp:

> 一旦你决定了该文件的目录，你需要创建一个防冲突的文件名。您也可以将路径保存在成员变量中供以后使用。下面是一个方法中的示例解决方案，该方法使用日期时间戳为新照片返回唯一文件名:

```
String mCurrentPhotoPath;

private File createImageFile() throws IOException {
    // Create an image file name
    String timeStamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
    String imageFileName = "JPEG_" + timeStamp + "_";
    File storageDir = getExternalFilesDir(Environment.DIRECTORY_PICTURES);
    File image = File.createTempFile(
        imageFileName,  /* prefix */
        ".jpg",         /* suffix */
        storageDir      /* directory */
    );

    // Save a file: path for use with ACTION_VIEW intents
    mCurrentPhotoPath = image.getAbsolutePath();
    return image;
}
```

With this method available to create a file for the photo, you can now create and invoke the Intent like this:

> 使用这种方法可以为照片创建文件，现在可以像这样创建和调用Intent：

```
static final int REQUEST_TAKE_PHOTO = 1;

private void dispatchTakePictureIntent() {
    Intent takePictureIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
    // Ensure that there's a camera activity to handle the intent
    if (takePictureIntent.resolveActivity(getPackageManager()) != null) {
        // Create the File where the photo should go
        File photoFile = null;
        try {
            photoFile = createImageFile();
        } catch (IOException ex) {
            // Error occurred while creating the File
            ...
        }
        // Continue only if the File was successfully created
        if (photoFile != null) {
            Uri photoURI = FileProvider.getUriForFile(this,
                                                  "com.example.android.fileprovider",
                                                  photoFile);
            takePictureIntent.putExtra(MediaStore.EXTRA_OUTPUT, photoURI);
            startActivityForResult(takePictureIntent, REQUEST_TAKE_PHOTO);
        }
    }
}
```

> Note: We are using getUriForFile(Context, String, File) which returns a content:// URI. For more recent apps targeting Android 7.0 (API level 24) and higher, passing a file:// URI across a package boundary causes a FileUriExposedException. Therefore, we now present a more generic way of storing images using a FileProvider.

> 注意：我们使用返回content：// URI的getUriForFile（Context，String，File）。对于更多针对Android 7.0（API级别24）及更高版本的应用，跨文件包边界传递file：// URI会导致FileUriExposedException。
>
> 因此，我们现在提出一种使用FileProvider存储图像的更通用的方法。

Now, you need to configure the FileProvider. In your app's manifest, add a provider to your application:

> 现在，您需要配置FileProvider。在您的应用清单中，向您的应用添加提供商：

```
<application>
   ...
   <provider
        android:name="android.support.v4.content.FileProvider"
        android:authorities="com.example.android.fileprovider"
        android:exported="false"
        android:grantUriPermissions="true">
        <meta-data
            android:name="android.support.FILE_PROVIDER_PATHS"
            android:resource="@xml/file_paths"></meta-data>
    </provider>
    ...
</application>
```

Make sure that the authorities string matches the second argument to getUriForFile(Context, String, File). In the meta-data section of the provider definition, you can see that the provider expects eligible paths to be configured in a dedicated resource file, res/xml/file_paths.xml. Here is the content required for this particular example:

> 确保权限字符串与getUriForFile（Context，String，File）的第二个参数匹配。在提供程序定义的元数据部分中，您可以看到提供程序希望将合格的路径配置到专用资源文件res / xml / file_paths.xml中。以下是此特定示例所需的内容：

```
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <external-path name="my_images" path="Android/data/com.example.package.name/files/Pictures" />
</paths>
```

The path component corresponds to the path that is returned by getExternalFilesDir() when called with Environment.DIRECTORY_PICTURES. Make sure that you replace com.example.package.name with the actual package name of your app. Also, checkout the documentation of FileProvider for an extensive description of path specifiers that you can use besides external-path.

> 路径组件对应于使用Environment.DIRECTORY_PICTURES调用时由getExternalFilesDir（）返回的路径。确保将com.example.package.name替换为应用程序的实际包名称。此外，请检查FileProvider的文档以获取除外部路径之外可以使用的路径说明符的广泛描述.

#### Add the photo to a gallery(将照片添加到画廊)

When you create a photo through an intent, you should know where your image is located, because you said where to save it in the first place. For everyone else, perhaps the easiest way to make your photo accessible is to make it accessible from the system's Media Provider.

> 当您通过意图创建照片时，您应该知道您的图像所在的位置，因为您说过首先将其保存在哪里。对于其他人而言，让您的照片易于访问的最简单方法是让系统的媒体提供商访问它。

> Note: If you saved your photo to the directory provided by getExternalFilesDir(), the media scanner cannot access the files because they are private to your app.

> 注意：如果您将照片保存到getExternalFilesDir（）提供的目录中，则媒体扫描程序无法访问这些文件，因为它们对您的应用程序是私人的。

The following example method demonstrates how to invoke the system's media scanner to add your photo to the Media Provider's database, making it available in the Android Gallery application and to other apps.

> 以下示例方法演示了如何调用系统的媒体扫描器将照片添加到媒体提供程序的数据库，使其可在Android Gallery应用程序和其他应用程序中使用。

```
private void galleryAddPic() {
    Intent mediaScanIntent = new Intent(Intent.ACTION_MEDIA_SCANNER_SCAN_FILE);
    File f = new File(mCurrentPhotoPath);
    Uri contentUri = Uri.fromFile(f);
    mediaScanIntent.setData(contentUri);
    this.sendBroadcast(mediaScanIntent);
}
```

##### Decode a scaled image(解码缩放的图像)

Managing multiple full-sized images can be tricky with limited memory. If you find your application running out of memory after displaying just a few images, you can dramatically reduce the amount of dynamic heap used by expanding the JPEG into a memory array that's already scaled to match the size of the destination view. The following example method demonstrates this technique.

> 管理多张全尺寸图像可能会因内存有限而变得棘手。如果在显示一些图像后发现应用程序内存不足，您可以通过将JPEG扩展到已调整大小以匹配目标视图大小的内存阵列来大幅减少动态堆的使用量。以下示例方法演示了这种技术。

```
private void setPic() {
    // Get the dimensions of the View
    int targetW = mImageView.getWidth();
    int targetH = mImageView.getHeight();

    // Get the dimensions of the bitmap
    BitmapFactory.Options bmOptions = new BitmapFactory.Options();
    bmOptions.inJustDecodeBounds = true;
    BitmapFactory.decodeFile(mCurrentPhotoPath, bmOptions);
    int photoW = bmOptions.outWidth;
    int photoH = bmOptions.outHeight;

    // Determine how much to scale down the image
    int scaleFactor = Math.min(photoW/targetW, photoH/targetH);

    // Decode the image file into a Bitmap sized to fill the View
    bmOptions.inJustDecodeBounds = false;
    bmOptions.inSampleSize = scaleFactor;
    bmOptions.inPurgeable = true;

    Bitmap bitmap = BitmapFactory.decodeFile(mCurrentPhotoPath, bmOptions);
    mImageView.setImageBitmap(bitmap);
}
```

