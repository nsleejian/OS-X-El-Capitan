###OS X El Capitan 更新速度慢的解决方法
####基本原理
 ---
#####抓去苹果官方下载链接，用第三方下载工具下载 .pkg 文件。搭建本地 server ,将苹果的下载地址解析到本地的服务器,从而实现快速下载安装。
 ---


(适用于 El Capitan 更新慢的用户，有百兆光纤的用户可忽略)

#####第一步、抓去苹果官方下载链接

由于前段时间的 Xcode Ghost 时间影响，当然不能再信任第三方的网盘，自己动手丰衣足食。
通过 Charles 抓取到的链接如下：

```
http://osxapps.itunes.apple.com/apple-assets-us-std-000001/Purple3/v4/74/d2/82/74d28291-9db9-7ae2-305d-9b8b3f5fd463/ftk3252456602304584541.pkg

```

####第二步、下载 .pkg文件
抓去到苹果官方的下载链接后，我们可以通过第三方下载工具下载 .pkg 文件,下载工具自选。下载完成后文件是无法直接安装的，下面我们要做的就是搭建本地 server 用来替换苹果官方的 server。

####第三步、搭建本地 server
* 3.1分析 URL 文件
   一般的 URL 格式如下：
   
   ```
   protocol :// hostname[:port] / path / [;parameters][?query]#fragment
   ```
   在这里我们只需要找到 hostname 和  path
         
   ```
   hostname  : osxapps.itunes.apple.com
   ```
   
   ```
   path      : /apple-assets-us-std-000001/Purple3/v4/74/d2/82/74d28291-9db9-7ae2-305d-9b8b3f5fd463/
   
   ```
* 3.2 将域名指向本地
   通过修改 hosts 文件 将域名指向 127.0.0.1，通过如下命令打开 hosts 文件
   
   ```
    sudo vim /etc/hosts  
   ```
   
   修改部分如下
   ![](http://ww1.sinaimg.cn/large/ad695ba9gw1ewlvkyv902j20yc0c8dk2.jpg)
   这样 DNS 就会将 osxapps.itunes.apple.com 解析到本地
  
* 3.3 搭建 SimpleHTTPServer 
   首先创建根目录(路径可自己修改)，本人放在桌面
   
   ```
   cd Desktop  
   mkdir elCapitanRoot 
   ```
   根据抓包得到的 path 创建相关路径,在 elCapitanRoot 目录下
   
   ```
   cd elCapitanRoot
   sudo mkdir -p  ./apple-assets-us-std-000001/Purple3/v4/74/d2/82/74d28291-9db9-7ae2-305d-9b8b3f5fd463/
   ```
   
    把下载的 .pkg 文件拷贝到路径下面，就是目录 74d28291-9db9-7ae2-305d-9b8b3f5fd463 下
![](http://ww4.sinaimg.cn/large/ad695ba9gw1ewlxhs5hwsj20yk02o3zg.jpg)    
    
    然后再 elCapitanRoot 目录下，注意一定要再 elCapitanRoot 目录下，运行如下命令启动 SimpleHTTPServer
    
    ```
    sudo python -m SimpleHTTPServer 80
    ```
    运行成功会如下图显示
    
![](http://ww3.sinaimg.cn/large/ad695ba9gw1ewlw5758ttj20uk04ita2.jpg)   

此时你可以在浏览器中测试一下儿下载速度，再地址栏输入我们抓去的苹果官方的下载链接，如果一切配置都正确，你应开回看到百兆的下载速度

![](http://ww3.sinaimg.cn/large/ad695ba9gw1ewlwa88cvzj20wc046myd.jpg)
####最后一步
打开 App Store 找到 OS X El Capitan 的更新按钮 ,点击更新按钮,稍等片刻，下载完成后就会弹出安装页面。安装过程需要一段时间,冲杯咖啡等待即可。

enjoy...




---
相关链接：

[URL](https://zh.wikipedia.org/wiki/%E7%BB%9F%E4%B8%80%E8%B5%84%E6%BA%90%E5%AE%9A%E4%BD%8D%E7%AC%A6)

[SimpleHTTPServer](http://coolshell.cn/articles/1480.html)

[hosts 文件](https://zh.wikipedia.org/wiki/Hosts%E6%96%87%E4%BB%B6)

