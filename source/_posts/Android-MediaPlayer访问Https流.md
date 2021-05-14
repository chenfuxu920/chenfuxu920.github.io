---
title: Android MediaPlayer访问Https流
toc: true
date: 2021-05-14 16:15:17
tags: [Android, Https, TLS]
categories: Android
---
现有项目出于安全考虑，使用了`Https双向认证`，这就要求客户端访问服务端资源时必须提供客户端证书，在使用`OkHttpClient`时，我们可以通过`OkHttpClient`配置项来设置`SSLSocketFactory`、`X509TrustManager`、`HostnameVerifier`，而`MediaPlayer`并没有提供配置网络请求的方法，只能通过`setDataSource(Uri)`来提供资源地址，但其实`MediaPlayer`内部也是通过`HttpsURLConnection`进行的网络访问，我们无法单独配置这一次网络请求，但我们可以配置`HttpsURLConnection`的默认请求。

通过`HttpsURLConnection.setDefaultSSLSocketFactory`和`HttpsURLConnection.setDefaultHostnameVerifier`来指定默认Https证书和域名认证即可
``` java
try {
	HttpsURLConnection.setDefaultSSLSocketFactory(
		SSLHelper.getSSLCertifcation(MyApplication.getContext()));
	HttpsURLConnection.setDefaultHostnameVerifier(new UnSafeHostnameVerifier());
} catch (Exception e) {
	e.printStackTrace();
}
```