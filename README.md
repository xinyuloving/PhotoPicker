# PhotoPicker
[![](https://jitpack.io/v/yudu233/PhotoPicker.svg)](https://jitpack.io/#yudu233/PhotoPicker)
Android 照片选择器 
简书：[PhotoPicker](http://www.jianshu.com/p/a6b5831797d0)

### 各位读者来点Star支持一下吧  传送门：[PhotoPicker]

参考项目：
- https://github.com/donglua/PhotoPicker
- https://github.com/YancyYe/GalleryPick

### PhotoPicker功能介绍
- 自定义图片加载方式
- 图片单选、多选
- 图片预览
- 自定义主题颜色
- 集成UCrop裁剪

### 效果预览
![01.png](http://upload-images.jianshu.io/upload_images/548993-cb27d44902920aed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![02.png](http://upload-images.jianshu.io/upload_images/548993-3bfb635af8fd49fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![03.png](http://upload-images.jianshu.io/upload_images/548993-3cb12cb642bbf270.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![04.png](http://upload-images.jianshu.io/upload_images/548993-8ed048f6cec25939.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![send.gif](http://upload-images.jianshu.io/upload_images/548993-2b9a11802bfc33d1.gif?imageMogr2/auto-orient/strip)
![circleimage.gif](http://upload-images.jianshu.io/upload_images/548993-20da4901efbe291e.gif?imageMogr2/auto-orient/strip)
![lookbigimage.gif](http://upload-images.jianshu.io/upload_images/548993-4027b93e6be8d593.gif?imageMogr2/auto-orient/strip)

### 用法
```java
allprojects {
    repositories {
        maven { url 'https://jitpack.io' }
        jcenter()
    }
}

```

在APP目录下的build.gradle中添加依赖

```java
    compile 'com.github.yudu233:PhotoPicker:1.0.0'
    
```

### AndroidManifest.xml 配置
```java
        <activity
            android:name=".ui.PhotoPickActivity"
            android:theme="@style/PhoAppTheme.AppTheme" />

        <activity
            android:name=".ui.PhotoPreviewActivity"
            android:theme="@style/PhoAppTheme.AppTheme" />

        <activity
            android:name="com.yalantis.ucrop.UCropActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>

        <activity android:name=".lookBigImage.ViewBigImageActivity"
                  android:screenOrientation="portrait"
                  android:theme="@style/Theme.AppCompat.Light.NoActionBar" />
                  
```

### 照片选择器配置
```java
    new PhotoPickConfig.Builder(this)
        .imageLoader(new GlideImageLoader())        //图片加载方式（必须）
        .showCamera(true)                           //是否显示拍照按钮（默认false）
        .clipPhoto(false)                           //是否裁剪图片（默认false）
        .clipCircle(true)                           //裁剪方式（默认矩形）
        .maxPickSize(9)                             //最多可选择图片个数（默认9张）
        .pickMode(PhotoPickConfig.MODE_PICK_MORE)   //手动设置照片多选还是单选（1单选2多选）
        .spanCount(3)                               //手动设置GridView列数（默认3列）
        .build();

```

### 配置说明
>  imageLoader(new GlideImageLoader())        //图片加载方式
    图片加载方式可以自定义支持Glide、Piscasso、Fresco

> clipCircle(true)                           //裁剪方式（默认矩形）
    当打开裁剪功能时可选择裁剪方式，默认为矩形，设置为true时裁剪方式为圆形，适用于设置用户头像。
    这里裁剪功能使用了开源的uCrop，暂时还未实现可自定义。

> 其他相关配置就不做过多介绍了，使用起来很方便，源码阅读起来也很简单
    完全可以自己修改成自己需要的样子。
    
### 自定义主题颜色，同步App主题
```java
        在Application里初始化PhotoPick
        //默认颜色：橘色android.R.color.holo_red_light
        PhotoPick.init(getApplicationContext());
        //自定义主题颜色
        PhotoPick.init(context, android.R.color.holo_red_light);    
        
```

### Activity 回调
```java
    @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (resultCode != RESULT_OK) return;
        switch (requestCode) {
            case PhotoPickConfig.PICK_REQUEST_CODE:
                //如果是裁剪返回一张就不加入集合了所以这里多了判断
                if (PhotoPickConfig.photoPickBean.isClipPhoto()){
                    Uri resultUri = Uri.parse(data.getStringExtra(PhotoPickConfig.EXTRA_CLIP_PHOTO));
                    //圆形图片加载
                    Glide.with(MainActivity.this).load(resultUri)
                            .transform(new GlideCircleTransform(this))
                            .into(mPic);
                }else {
                    ArrayList<String> photoLists = data.getStringArrayListExtra(PhotoPickConfig.EXTRA_STRING_ARRAYLIST);
                    if (photoLists != null && !photoLists.isEmpty()) {
                        for (int i = 0; i < photoLists.size(); i++)
                            if (mImagePaths.size() < 9)
                                mImagePaths.add(photoLists.get(i));
                    }
                    mAddPicture.setPaths(mImagePaths);
                }
                break;
        }
    }
    
```
有问题可以添加QQ：19880794 


