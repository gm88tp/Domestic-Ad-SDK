# Android 国内 子SDK 变现广告

### 子sdk简述
    
   广告子SDK主要负责集成聚合、穿山甲、优量汇第三方的广告sdk，统一实现播放以及播放完毕后的回调。
   
   该子sdk需依赖于主SDK 3.8.8.3以上版本
  

### sdk远端依赖引用方式

   在project的build文件下添加maven库地址
   ```
   allprojects {
       repositories {
   
   //        广告子sdk maven库
           maven { url 'https://raw.githubusercontent.com/gm88tp/Domestic-Ad-SDK/master' }
   
   //        穿山甲sdk maven库
           maven { url 'https://artifact.bytedance.com/repository/pangle' }
           
           }
   
   ```
   
   在module的build文件下添加依赖(最后版本号按情况自行调整)
   ```
   dependencies {
       implementation 'com.qq.e.union:union:4.370.1240'  //优量汇
       implementation 'com.pangle.cn:ads-sdk:3.6.1.3' //穿山甲
       implementation 'com.gm88:sdkad:3.8.8.3' //广告子sdk
   }
   ```
   
   Manifest中注册以下第三方广告sdk需要的provider
   ```
        <provider
            android:name="com.qq.e.comm.GDTFileProvider"
            android:authorities="${applicationId}.gdt.fileprovider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/gdt_file_path" />
        </provider>

       <provider
            android:name="com.bytedance.sdk.openadsdk.TTFileProvider"
            android:authorities="${applicationId}.TTFileProvider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_paths" />
        </provider>

        <provider
            android:name="com.bytedance.sdk.openadsdk.multipro.TTMultiProvider"
            android:authorities="${applicationId}.TTMultiProvider"
            android:exported="false" />
   
   ```
   
   
   xml资源下新建添加file_Path.xml
   
   ```
   <?xml version="1.0" encoding="utf-8"?>
    <resources>
    <paths xmlns:android="http://schemas.android.com/apk/res/android">
    <!--为了适配所有路径可以设置 path = "." -->
    <external-path name="tt_external_root" path="." />
    <external-path name="tt_external_download" path="Download" />
    <external-files-path name="tt_external_files_download" path="Download" />
    <files-path name="tt_internal_file_download" path="Download" />
    <cache-path name="tt_internal_cache_download" path="Download" />
    </paths>
    </resources>
   
   ```
   
    

### 方法调用

   初始化 在调用广告之前请先进行初始化，在应用Application的onCreate方法内调用以下方法。
   ADMix.initApp(MyApplication.this);
   
   在主Activity内，怪猫SDK初始化成功的回调内，调用以下方法
   
   
   
   ```
   ADMix.initMainActivity(MainActivity.this);
   ```
   
   在调用广告之前请先设置广告回调，请在主Activity的初始化之前进行回调设置
   ```
   ADMix.setAdCallBack(new AdCallBack() {
   @Override
   public void onAdFailed(String errormsg) {
      Toast.makeText(MainActivity.this,"onAdFailed",Toast.LENGTH_LONG).show();
   }
   
   @Override
   public void onVideoComplete(String extra) {
      Toast.makeText(MainActivity.this,"onVideoComplete",Toast.LENGTH_LONG).show()
   ;
   }
   })
   ```

   调用广告
   ```
   ADMix.showVideoAD(type,extra);
   ```
   
   | 字段  | 类型     | 说明            |
   | -----| ------ | ---------------- |
   | type   | int      | 目前固定值： 13 |
   | extra | string | 播放广告透传参数，在广告播放成功后，会原样回 |
