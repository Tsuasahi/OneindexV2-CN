# OneindexV2-CN

Onedrive Directory Index 世纪互联版
##
改自[oneindex2-in](https://github.com/lzx8589561/oneindex2-in)

## 功能:
不用服务器空间,不走服务器流量,  

直接列onedrive目录,文件直链下载.  

## demo
[https://cloud.tsuasahi.com](https://cloud.tsuasahi.com)  

## change log:  
20-2-20: 添加nexmoe主题
20-2-20: 添加ass|ssa|vtt字幕支持 
  
## 需求:
1. PHP空间,PHP 5.6+ 打开curl支持  
2. OneDrive For Business 世纪互联版账号 (企业版或教育版/工作或学校帐户)
3. Oneindex 程序   

## 安装:
1. 复制oneindex到服务器,设置` config/ ` `config/base.php`  `cache/` 可读写  
2. 访问[https://portal.azure.cn](https://portal.azure.cn) 
登录账号后在左边栏找到 `Azure Active Directory` -> `应用注册`
新注册应用,名称任意重定向URI填写站点地址
添加API权限 `Sharepoint` -> `委托的权限` -> `Files.Read & Read user files` 勾选后更新权限
获取应用ID和密钥,添加密钥时有效时期选择1年内,记下应用ID和密钥
进入 [世纪互联Office365主页](https://portal.partner.microsoftonline.cn/Home) ,登录后选择OneDrive
记下网址-my.sharepoint.cn前的内容,例如上面显示网址为example-my.sharepoint.cn,则记下example
3. 浏览器访问,分别填写`应用ID` `应用机密(密钥)` `回调地址(重定向URI)` `世纪互联域名前缀` 绑定账号
4. 略作等待,即可使用 

## 计划任务  
[可选]**推荐配置**,非必需。后台定时刷新缓存,可增加前台访问的速度  
`crontab -e` 进行编辑
```
# 每小时刷新一次token
0 * * * * /具体路径/php /程序具体路径/one.php token:refresh

# 每十分钟后台刷新一遍缓存
*/10 * * * * /具体路径/php /程序具体路径/one.php cache:refresh
```

## 特殊文件实现功能  
` README.md ` `HEAD.md` `.password`特殊文件使用  

可以参考[https://github.com/donwa/oneindex/tree/files](https://github.com/donwa/oneindex/tree/files)  

**在文件夹底部添加说明:**  
>在onedrive的文件夹中添加` README.md `文件,使用markdown语法.

**在文件夹头部添加说明:**  
>在onedrive的文件夹中添加`HEAD.md` 文件,使用markdown语法.

**加密文件夹:**  
>在onedrive的文件夹中添加`.password`文件,填入密码,密码不能为空.

## 命令行功能  
仅能在php cli模式下运行  
**清除缓存:**  
```
php one.php cache:clear
```
**刷新缓存:**  
```
php one.php cache:refresh
```
**刷新令牌:**  
```
php one.php token:refresh
```
**上传文件:**  
```
php one.php upload:file 本地文件 [onedrive文件]
```
例如:  
```
//上传demo.zip 到onedrive 根目录  
php one.php upload:file demo.zip  

//上传demo.zip 到onedrive /test/目录  
php one.php upload:file demo.zip /test/  

//上传demo.zip 到onedrive /test/目录并命名为 d.zip
php one.php upload:file demo.zip /test/d.zip  
```

## 可配置项
配置在 `config/base.php` 文件中:  

**onedrive共享的起始目录:**  
```
'onedrive_root'=> '', //默认为根目录
```  

如果想只共享onedrive下的 /document/share/ 目录  
```
'onedrive_root'=> '/document/share', //最后不带 '/'
```  
  
**去掉链接中的 /?/ :**  
需要添加apache/nginx/iis的rewrite的配置文件  
参考程序根目录下的:`.htaccess`或`nginx.conf`或`Web.config`  
```
  //在config/base.php 中
  'root_path' => '?' 
```
改为  

```
    'root_path' => '' 
```  
  
**缓存时间:**  
初步测试直链过期时间为一小时,默认设置为: 
```
  'cache_expire_time' => 3600, //缓存过期时间 /秒
  'cache_refresh_time' => 600, //缓存刷新时间 /秒
```
如果经常出现链接失效,可尝试缩短缓存时间,如:  
```
  'cache_expire_time' => 300, //缓存过期时间 /秒
  'cache_refresh_time' => 60, //缓存刷新时间 /秒
```




## Q&A:  
**Q: 需要企业版或教育版的全局管理员?**  
A: 不需要,全局管理员开出来的子账号就可以,不过该域名在office365必须要有管理员  

**Q: 文件上传后,不能即时在程序页面显示出来?**  
A: 有缓存,可以在config/base.php设置缓存时间. 或在后台刷新缓存

