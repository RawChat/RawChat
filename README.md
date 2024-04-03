# RawChat Shout Out To Pandora 
**一个利用反向代理使用ChatGPT官网的通用解决方案。**

RawChat的设计初衷是降低ChatGPT官网的使用门槛，支持的功能：  
1. 使用体验与官网完全一致，可在第一时间体验到官网所有新功能
2. 无需科学上网
3. 内置多个Plus账号，不用担心次数不够
4. 对话隔离，他人无法看到你的对话内容，保护隐私
5. 支持所有Plugin插件功能
6. 支持所有GPTs功能
7. 支持PDF、图片分析功能
8. 支持联网对话功能
9. 无需担心封号风险

RawChat技术原理：  
RawChat的技术栈选用反向代理解决方案，即RawChat作为中间人转发用户到官网的请求以及响应，从而实现免梯目的，并且RawChat会接管部分官网的功能，本地化部分接口（比如登录注册接口是由RawChat接管的，所以使用的不是官网账号），可以理解为您就在实时的使用官网。

[Rawchat使用教程文档](https://gqetpw6dpfw.feishu.cn/docx/Jc6idZvtRoEvxQxiF9Fcu4Hun8e)（商业站点运营文档，一定要先阅读文档！！）  

[Rawchat商业站点](https://chat.openai.fo)，成品演示（登录用的不是官网账号，需要注册）

[SharedChat共享站点](https://sharedchat.cn/shared.html)，免费提供多个Plus共享账号！  

[Rawchat直登站点](https://chat.rawchat.cc)，可以使用官网的账号直接登录

[Rawchat桌面版](https://t0svlmehz1v.feishu.cn/docx/UvWJd3e0Yozgt0xxXaXcW4uAnuf)，采用正向代理，“最后的防线”

**您也可以接入RawChat，让您的网站也拥有一样的功能(可以对接自己的卡网)：**

接入前置条件：  
1.您需要拥有自己的域名  
2.您需要拥有自己的服务器（linux、windows）都行  
3.您需要安装宝塔面板，方便操作  

接入步骤：  
假设域名为abc.com  
1.解析**chat.abc.com、tcr9i.chat.abc.com、auth0.abc.com**，一共需要解析三个A记录到您自己的服务器  
2.打开宝塔面板添加网站，将上面的三个网址添加到网站，如下图  
![image](https://github.com/RawChat/RawChat/assets/157953998/7cbb5ba3-d786-42c0-b29d-da766ca15f0b)  
3.将这三个网站都开启SSL证书，Let's Encrypt免费  
4.将这三个网站都开启反向代理，反向代理的域名为：**rawchat.fun**，替换对应的前缀就好了，如下图  
![image](https://github.com/RawChat/RawChat/assets/157953998/37856ca0-b533-4236-80f0-f4485bae5d64)  
反向代理配置，最好直接复制：  
```location /
{
    expires 12h;
    if ($request_uri ~* "(php|jsp|cgi|asp|aspx)")
    {
         expires 0;
    }
    proxy_pass https://（chat、tcr9i.chat、auth0替换成对应的前缀，一共三个网站）.rawchat.fun;
    proxy_set_header Host $proxy_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    proxy_set_header X-Host $host;
    proxy_buffering off;
    proxy_cache off;

    add_header X-Cache $upstream_cache_status;
    add_header Cache-Control "no-store, no-cache, must-revalidate, proxy-revalidate, max-age=0";
	add_header X-Cache $upstream_cache_status;

    proxy_set_header Accept-Encoding "";
	
    sub_filter_once off;

    #proxy_cache cache_one;
    #proxy_cache_key $host$uri$is_args$args;
    #proxy_cache_valid 200 304 301 302 12h;
}
```  
5.打开浏览器访问chat.abc.com，看到如下图则代表接入成功  
![image](https://github.com/RawChat/RawChat/assets/157953998/8c1b6e9d-9306-4780-a006-47f6087f012c)

如果您觉得RawChat好，想支持RawChat，请务必点个Star，谢谢！

**RawChat交流群：**  
![image](https://github.com/RawChat/RawChat/assets/157953998/59e5c73e-11cd-4f92-acd4-d7ea63847d7f)

