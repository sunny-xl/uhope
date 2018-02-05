# 企业版项目配置文件说明

## 实现原理

不通过在AppStore,在iOS设备上直接安装应用程序，其原理是通过*itms-services协议*，在safari浏览器可以直接在iOS设备上安装应用程序。itms-services协议需要一个plist配置文件

## 实现方法：

一、第一种方法：
   最简单便捷的方法，把项目托管到第三方平台上，如：[fir.im](fir.im)或者 [蒲公英托管平台](https://www.pgyer.com/)。<br/>
   这样的话，你只需要配置企业版证书，打包时类型选择企业版，导出ipa文件，然后上传到三方托管平台即可。<br/>
   缺点：1、三方托管平台免费版的话每天有下载次数限制。当然可以通过交费来解决这个问题。
   	2、文件都放到三方平台，安全性无法掌控，一旦三方平台挂了，那就悲剧了
	
二、第二种方法：
   相关文件放到自己服务器上，自己掌控。

   需要文件：plist文件、ipa文件、57x57和512x512的图片、html下载安装页面。

### plist文件


1. plist文件必须放到 https的服务器上了，http不可以用。plist文件可以通过xcode进行企业版本打包时自动生成
    - 直接放到公司的https的网站上
    - 找一个第三方https外链的网盘，将plist文件放到网盘上，ipa安装包可以放在自己的服务器上。

2. plist文件和ipa文件名字必须相同
3. 访问plist文件时需要直接以文本形式读取，因此需要设定web服务器 MIME 类型
    - 对于 OS X Server，将以下 MIME 类型添加到 Web 服务的“MIME Types”（MIME 类型）设置：
```
    application/octet-stream ipa
    text/xml plist
```
    - 对于 IIS，使用 IIS Manager 在服务器的“属性”页面中添加 MIME 类型：
```
    .ipa application/octet-stream
    .plist text/xml
```

4. plist文件里面具体内容填写说明

    URL : 应用程序 (.ipa) 文件的完整合格的地址，http或者https都可以 <br />
    display-image：下载和安装过程中显示的57 x 57像素PNG图像。指定图像的完整合格的 URL。不可缺少  <br />
    full-size-image：用来在 iTunes 中表示应用程序的512 x 512像素PNG图像。不可缺少  <br />
    bundle-identifier：您应用程序的包标识符，即appid ,与 Xcode 项目中指定的完全一样。  <br />
    bundle-version: 您应用程序的包版本，在 Xcode 项目中指定。  <br />
    title: 下载和安装过程中显示的应用程序的名称。  <br />  <br />

    *还可以根据需要自定义字段*
    如我根据项目需要，添加了每次版本更新的更新提示内容  <br />

    ```
    <key>updateMsg</key>
	<string>1.1.0版本发布了，优化了用户体验</string>
    ```

### ipa文件
项目安装包，配置企业版证书，用xcode打包，选择类型时要选择企业版，导出ipa文件

### 57和512图片
下载和安装过程中会显示的57 x 57图像，在iTunes中表示应用程序的512 x 512图像，放到https或者http上都可以

### html安装文件
里面最主要的部分 ：itms-services://?action=download-manifest&url= *项目plist文件的地址*

其他内容自己定制，可以设置成点击按钮开始下载，也可以设置成进入这个页面直接触发下载。比如我上传的html文件是点击安装按钮后开始下载。
