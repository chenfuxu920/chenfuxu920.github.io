---
title: 使用系统裁剪时FileProvider权限问题
date: 2017.08.16 12:35:32
tags: [Android, FileProvider]
toc: true
---

### FileProvider使用
当使用FileProvider生成uri时，使用uri的intent应添加
``` java
intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
intent.addFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION);
```
来授予权限。

### 使用系统裁剪时遇到的坑
``` java
Intent intent = new Intent("com.android.camera.action.CROP");
intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
intent.addFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION);
intent.setDataAndType(uri, "image/*");     
intent.putExtra("crop", "true");
intent.putExtra("return-data", false);
intent.putExtra(MediaStore.EXTRA_OUTPUT, tempUri);
startActivityForResult(intent,REQUEST_CODE_CUT_PHOTO);
```
当`intent.setDataAndType(uri,"image/*");`中的 uri 和 `intent.putExtra(MediaStore.EXTRA_OUTPUT,tempUri);` 中 tempUri 一致时，使用`intent.addFlags`即可，但当 uri 和 tempUri 不一致时则会报错
``` java
java.lang.SecurityException: Permission Denial: opening provider android.support.v4.content.FileProvider from ProcessRecord，
```

这是因为`intent.addFlags`只是为intent访问uri添加了权限，没有为访问tempUri添加权限，所以保存裁剪图时会报错，这时必须使用如下代码来为intent添加tempUri权限
``` java
List resInfoList = getActivity().getPackageManager().queryIntentActivities(intent,PackageManager.MATCH_DEFAULT_ONLY);
for(ResolveInfo resolveInfo : resInfoList) {
    String packageName = resolveInfo.activityInfo.packageName;
    getActivity().grantUriPermission(packageName,tempUri,Intent.FLAG_GRANT_WRITE_URI_PERMISSION| 
    Intent.FLAG_GRANT_READ_URI_PERMISSION);
}
```