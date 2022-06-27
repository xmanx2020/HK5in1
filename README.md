### 关注甬哥项目https://gitlab.com/rwkgyg/

### 请下载以上代码，使用Github进行代码托管并连接heroku
![c14a9fd3dfe0361393dcc7cd693cc36](https://user-images.githubusercontent.com/107276912/173172932-142f6c9a-7f7f-424b-a178-aa43772a7511.png)


### workers反代与pages反代及自定义域，配置文件信息等相关操作拓展教程，请关注：[博客视频教程](https://ygkkk.blogspot.com/2022/05/heroku-cloudflare-workers-pages.html)


### CloudFlare Workers反代代码（可分别用两个账号的应用程序名（`path路径`、`协议`、`UUID`保持一致），单双号天分别执行，那一个月就有550+550小时（每个账号一个月免费使用550小时））
<details>
<summary>CloudFlare Workers单账户反代代码</summary>

```js
addEventListener(
    "fetch",event => {
        let url=new URL(event.request.url);
        url.hostname="appname.herokuapp.com";
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>CloudFlare Workers单双日轮换反代代码</summary>

```js
const SingleDay = 'app0.herokuapp.com'
const DoubleDay = 'app1.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>CloudFlare Workers每五天轮换一遍式反代代码</summary>

```js
const Day0 = 'app0.herokuapp.com'
const Day1 = 'app1.herokuapp.com'
const Day2 = 'app2.herokuapp.com'
const Day3 = 'app3.herokuapp.com'
const Day4 = 'app4.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        let day = nd.getDate() % 5;
        if (day === 0) {
            host = Day0
        } else if (day === 1) {
            host = Day1
        } else if (day === 2) {
            host = Day2
        } else if (day === 3){
            host = Day3
        } else if (day === 4){
            host = Day4
        } else {
            host = Day1
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>CloudFlare Workers一周轮换反代代码</summary>

```js
const Day0 = 'app0.herokuapp.com'
const Day1 = 'app1.herokuapp.com'
const Day2 = 'app2.herokuapp.com'
const Day3 = 'app3.herokuapp.com'
const Day4 = 'app4.herokuapp.com'
const Day5 = 'app5.herokuapp.com'
const Day6 = 'app6.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        let day = nd.getDay();
        if (day === 0) {
            host = Day0
        } else if (day === 1) {
            host = Day1
        } else if (day === 2) {
            host = Day2
        } else if (day === 3){
            host = Day3
        } else if (day === 4) {
            host = Day4
        } else if (day === 5) {
            host = Day5
        } else if (day === 6) {
            host = Day6
        } else {
            host = Day1
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

=========================================================
一、Heroku节点应用建立操作步骤：

1、在浏览器复制链接   https://dashboard.heroku.com/new?template= 加上上传至Github的项目地址链接，回车进入Heroku参数设置界面



2、之前没有登录记录的话，会先提示注册并或登录Heroku界面，大家自己注册或者登录下



3、Heroku app名称与国家随意，最后设置图如下



4、输入UUID是必输项，别的可以不用改。建议使用V2rayN等工具生成，点击Deploy app，几秒种后就完成安装。



5、与视频教程有出入的地方：由于更新后使用了Caddy，未设伪装网页，所以点击heroku本地域名时（app.heroku.com）为空白界面，所有反代（workers与pages）测试为绿色200 ok，反代地址也为空白界面，请大家知晓。



6、关于heroku封杀特征说明（甬哥已亲身经历过以下三次被封）：

一、项目部署时被强制中断封杀

二、项目部署后，测个速度也会被封杀

三、项目部署后，不跑流量，放着不动也会被封杀

已证实，heroku封杀看特性，不看流量大小。之前的项目是明文，现在加密了，测试了几周没问题，所以大家可以试试看。

-------------------------------------------------------------------------------------------

二、关于为什么套CF以及满足自选IP/域名的条件解答（443端口，且TLS开启）



三、客户端配置如下

IOS端小火箭就可以通吃，安卓端推荐V2rayNG或搭配Kitsunebi

协议：(vless/vmess/trojan)-ws

地址：app.heroku.com（自选IP/域名）

端口：443

用户ID/密码：自定义UUID

传输协议：ws

伪装host：app.heroku.com（workers或pages反代/自定义域）

路径path：/自定义UUID-协议开头两小写字母

传输安全：tls

SNI：app.heroku.com（workers或pages反代/自定义域）

其他设置保持默认即可！



shadowsocks-ws与socks5-ws推荐用Kitsunebi，配置简单，不需要plugin插件

协议：(shadowsocks/socks5)-ws

地址：app.heroku.com（自选IP/域名）

端口：443

shadowsocks密码：自定义UUID

shadowsocks加密方式：chacha20-ietf-poly1305(默认)

socks5用户名：空

socks5密码：空

传输协议：ws

伪装host：app.heroku.com（workers或pages反代/自定义域）

路径path：/自定义UUID-协议开头两小写字母

传输安全：tls

SNI(证书域名)：app.heroku.com（workers或pages反代/自定义域）

其他设置保持默认即可！
=========================================================

### 感谢以下项目所提供的参考

https://github.com/mixool/xrayku  （已删库）

https://github.com/Cptmacmillan2022007/IX-X2VW

