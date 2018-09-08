---
layout: post
title: "物联-android实现升级后自启动"
date: 2018-09-08 12:00:00
description: "android应用在检测更新安装后实现自启动"
tag: android
---
     

### 难点与解决思路

1. 检测更新：
    ````
    检测更新接口
    path:/apk/update
    method:POST
    -------param------
    {
      "versionCode" : "10000",
      "channl"  : "dev"
    }
    -----return-----
    {
      "code" : "000000",
      "data" : {
            "url":"",
            "versionCode":"10001",
            "versionName":"dev",
            "des":"更新描述"
            "md5":""
      }
      "msg" : "success"
    }
    ````
2. 下载，我这里用的rx2-android-networking来下载的

    ````
    public void download(final ApkInfo apkInfo) {
        final File dest = new File(context.getCacheDir(), apkInfo.getVersionCode() + ".apk");
        if (verifyInstallApk(apkInfo, dest)) {
            return;
        }
        final File temp = new File(context.getCacheDir(), apkInfo.getVersionCode() + ".apk.temp");
        AndroidNetworking.download(apkInfo.getUrl(), temp.getParent(), temp.getName())
                .build()
                .startDownload(new DownloadListener() {
                    @Override
                    public void onDownloadComplete() {
                        if (dest.exists()) dest.delete();
                        temp.renameTo(dest);
                        verifyInstallApk(apkInfo, dest);
                    }

                    @Override
                    public void onError(ANError anError) {
                        AppLogger.e("apk更新失败" + anError.getErrorDetail());
                    }
                });
    }

    ````
    
3. apk文件校验
    ````
    private boolean verifyInstallApk(ApkInfo apkInfo, File dest) {
        if (dest.exists()) {
            String md5 = EncryptUtils.encryptMD5File2String(dest);
            if (apkInfo.getMd5().equals(md5)) {
                installApk(dest);
                return true;
            }
        }
        return false;
    }

    ````
4. 安装
    1. 普通安装
        ````
        private void installApp(File file) {
            if (file == null) return;
            Intent intent = new Intent(Intent.ACTION_VIEW);
            Uri data;
            String type = "application/vnd.android.package-archive";
            if (Build.VERSION.SDK_INT < Build.VERSION_CODES.N) {
                data = Uri.fromFile(file);
            } else {
                intent.setFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
                String authority = Utils.getApp().getPackageName() + ".utilcode.provider";
                data = FileProvider.getUriForFile(Utils.getApp(), authority, file);
            }
            intent.setDataAndType(data, type);
            intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            context.startActivity(intent);
        }
        ````
    2. 静默安装
    
        ````
        public static boolean installSlient(String path) {
            String cmd = String.format("pm install -r %s", path);
            Process process = null;
            DataOutputStream os = null;
            BufferedReader successResult = null;
            BufferedReader errorResult = null;
            StringBuilder successMsg = new StringBuilder();
            StringBuilder errorMsg = new StringBuilder();
            try {
                //静默安装需要root权限
                process = Runtime.getRuntime().exec(cmd);
                os = new DataOutputStream(process.getOutputStream());
                os.flush();
                process.waitFor();
                successResult = new BufferedReader(new InputStreamReader(process.getInputStream()));
                errorResult = new BufferedReader(new InputStreamReader(process.getErrorStream()));
                String s;
                while ((s = successResult.readLine()) != null) {
                    successMsg.append(s);
                    AppLogger.d("installSlient success", "successMsg = " + s);
                }
                while ((s = errorResult.readLine()) != null) {
                    AppLogger.d("installSlient error", "errorResult = " + s);
                    errorMsg.append(s);
                }
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                try {
                    if (os != null) {
                        os.close();
                    }
                    if (process != null) {
                        process.destroy();
                    }
                    if (successResult != null) {
                        successResult.close();
                    }
                    if (errorResult != null) {
                        errorResult.close();
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            return successMsg.toString().equalsIgnoreCase("SUCCESS");
        }
        ````
  

### 更新后自启动

<br>

转载请注明：[小嵩的博客](http://changs.top) » [点击阅读原文](http://changs.top/2018/09/android-upgrade-runself/)
