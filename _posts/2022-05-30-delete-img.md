---
layout: post
title:  "删除设备中的文件，并通知媒体库更新"
date:   2022-05-30 15:21:28 +0801
categories: articles
tags: [problem]
---
删除设备中图片文件，通知媒体库(相册)更新

<br>

### 迭代删除图片删除文件
```text
    public static boolean delete() {
        try {
            File file = getYoursRootImgFolder(); // 存放图片文件的根目录
            if (file.exists() && file.isDirectory()) {
                deleteFile(file);
            }
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
        return true;
    }

    private static void deleteFile(File file) {
        try {
            File[] files = file.listFiles();
            if (file.listFiles() == null || file.listFiles().length <= 0) return;

            for (File child : files) {
                if (child.isFile()) {
                    if (isCanDelete(child)) {
                        child.delete();
                    }
                } else if (child.isDirectory()) {
                    deleteFile(child);
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

### 通过内容提供者(ContentResolver)删除图片 --(后面的不知道效果咋样)
```text
Uri uri = MediaStore.Images.Media.EXTERNAL_CONTENT_URI;
String where = MediaStore.Images.Media.DATA + "='" + filePath + "'"; // filePath 文件路径
context.getContentResolver().delete(uri, where, null);
```
`where`语句也可以写成这样(试试)：
```text
String where = MediaStore.Audio.Media.DATA+" like \"" + filepath + "%" + "\"";
```

### 更新媒体库
```text
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
    new MediaScanner(PreviewActivity.this, path);
} else {
    sendBroadcast(new Intent(Intent.ACTION_MEDIA_MOUNTED, Uri.parse("file://" + Environment.getExternalStorageDirectory())));
}
```
如果MediaScanner类不存在，尝试使用下面的方法
```text
public static void updateMediaStore(final  Context context, final String path) {
    //版本4.4以下发送广播更新媒体库
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT){
        MediaScannerConnection.scanFile(context, new String[]{path}, null, new MediaScannerConnection.OnScanCompletedListener() {
            public void onScanCompleted(String path, Uri uri) {
                Intent mediaScanIntent = new Intent(Intent.ACTION_MEDIA_SCANNER_SCAN_FILE);
                mediaScanIntent.setData(uri);
                context.sendBroadcast(mediaScanIntent);
            }
        });
    } else {
        File file = new File(path);
        String relationDir = file.getParent();
        File file1 = new File(relationDir);
        context.sendBroadcast(new Intent(Intent.ACTION_MEDIA_MOUNTED, Uri.fromFile(file1.getAbsoluteFile())));
    }
}
```