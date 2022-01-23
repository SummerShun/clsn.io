---
title: HTTPS之acme.sh申请证书
author: 惨绿少年
type: post
date: 2018-11-29T11:22:45+00:00
url: /clsn/lx1461.html
Baidusubmit:
  - 1
custom_title:
  - HTTPS之acme.sh申请证书
description:
  - "Let's Encrypt是一个于2015年三季度推出的数字证书认证机构，旨在以自动化流程消除手动创建和安装证书的复杂流程，并推广使万维网服务器的加密连接无所不在，为安全网站提供免费的SSL/TLS证书。"
keywords:
  - https,acme.sh,clsn,惨绿少年
zm_like:
  - 1
categories:
  - Linux运维
  - 云计算
  - 安全
  - 敏捷开发
  - 玩转Linux
  - 运维基本功

---
<div id="log-box">
  <div id="catalog">
    <ul id="catalog-ul">
      <li>
        <i class="be be-arrowright"></i> <a href="#title-0" title="3.2.1 颁发证书">3.2.1 颁发证书</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-1" title="3.2.2 修改DNS">3.2.2 修改DNS</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-2" title="3.2.4 生成证书">3.2.4 生成证书</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-3" title="3.2.5 dns 方式API">3.2.5 dns 方式API</a>
      </li>
    </ul>
    
    <span class="log-zd"><span class="log-close"><a title="隐藏目录"><i class="be be-cross"></i><strong>目录</strong></a></span></span>
  </div>
</div>

## <span id="1lets_encryptacmesh">1.关于let's encrypt和acme.sh的简介</span>

### <span id="11_lets_encrypt">1.1 let's encrypt</span>

<img data-original="https://cdn.nlark.com/yuque/0/2018/png/206952/1542677138536-70f49093-46e1-4bcc-8494-2d1e289fdf8a.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="HTTPS之acme.sh申请证书" alt="图片.png | center | 349x86" title="" /> 

Let's Encrypt是一个于2015年三季度推出的数字证书认证机构，旨在以自动化流程消除手动创建和安装证书的复杂流程，并推广使万维网服务器的加密连接无所不在，为安全网站提供免费的SSL/TLS证书。  
Let's Encrypt由互联网安全研究小组（缩写ISRG）提供服务。主要赞助商包括电子前哨基金会、Mozilla基金会、Akamai以及思科。2015年4月9日，ISRG与Linux基金会宣布合作。  
用以实现新的数字证书认证机构的协议被称为自动证书管理环境（ACME）。GitHub上有这一规范的草案，且提案的一个版本已作为一个Internet草案发布。  
Let's Encrypt宣称这一过程将十分简单、自动化并且免费

### <span id="12_acmesh">1.2 acme.sh</span>

简单来说acme.sh 实现了 acme 协议, 可以从 let‘s encrypt 生成免费的证书。  
acme.sh 有以下特点：  
* 一个纯粹用Shell（Unix shell）语言编写的ACME协议客户端。  
* 完整的ACME协议实施。 支持ACME v1和ACME v2 支持ACME v2通配符证书  
* 简单，功能强大且易于使用。你只需要3分钟就可以学习它。  
* Let's Encrypt免费证书客户端最简单的shell脚本。  
* 纯粹用Shell编写，不依赖于python或官方的Let's Encrypt客户端。  
* 只需一个脚本即可自动颁发，续订和安装证书。 不需要root/sudoer访问权限。  
* 支持在Docker内使用，支持IPv6

## <span id="2acmesh">2.安装acme.sh</span>

### <span id="21">2.1 执行安装</span>

<https://github.com/Neilpang/acme.sh/wiki/dns-manual-mode>  
安装很简单, 一个命令:

<pre><code class="language-bash line-numbers">curl  https://get.acme.sh | sh
</code></pre>

脚本会根据系统不通选择不同的下载方式：

<img data-original="https://cdn.nlark.com/yuque/0/2018/png/206952/1542677480706-8ddeca43-e763-4acd-973f-1ad8b48a6df3.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="HTTPS之acme.sh申请证书" alt="图片.png | center | 747x370" title="" /> 

<pre><code class="language-bash line-numbers">[root@clsn.io /opt] 
#wget -O -  https://raw.githubusercontent.com/Neilpang/acme.sh/master/acme.sh | INSTALLONLINE=1  sh
--2018-11-13 15:31:42--  https://raw.githubusercontent.com/Neilpang/acme.sh/master/acme.sh
正在解析主机 raw.githubusercontent.com... 151.101.0.133, 151.101.64.133, 151.101.128.133, ...
正在连接 raw.githubusercontent.com|151.101.0.133|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：168608 (165K) [text/plain]
正在保存至: “STDOUT”

100%[======================================================================&gt;] 168,608     --.-K/s   in 0.07s

2018-11-13 15:31:42 (2.38 MB/s) - 已写入标准输出 [168608/168608]

[2018年 11月 13日 星期二 15:31:42 CST] Installing from online archive.
[2018年 11月 13日 星期二 15:31:42 CST] Downloading https://github.com/Neilpang/acme.sh/archive/master.tar.gz
[2018年 11月 13日 星期二 15:31:43 CST] Extracting master.tar.gz
[2018年 11月 13日 星期二 15:31:43 CST] It is recommended to install socat first.
[2018年 11月 13日 星期二 15:31:43 CST] We use socat for standalone server if you use standalone mode.
[2018年 11月 13日 星期二 15:31:43 CST] If you don't use standalone mode, just ignore this warning.
[2018年 11月 13日 星期二 15:31:43 CST] Installing to /root/.acme.sh
[2018年 11月 13日 星期二 15:31:43 CST] Installed to /root/.acme.sh/acme.sh
[2018年 11月 13日 星期二 15:31:43 CST] Installing alias to '/root/.bashrc'
[2018年 11月 13日 星期二 15:31:43 CST] OK, Close and reopen your terminal to start using acme.sh
[2018年 11月 13日 星期二 15:31:43 CST] Installing alias to '/root/.cshrc'
[2018年 11月 13日 星期二 15:31:43 CST] Installing alias to '/root/.tcshrc'
[2018年 11月 13日 星期二 15:31:43 CST] Installing cron job
#未知信息，已禁用 ：34 0 * * * "/root/.acme.sh"/acme.sh --cron --home "/root/.acme.sh" &gt; /dev/null
[2018年 11月 13日 星期二 15:31:43 CST] Good, bash is found, so change the shebang to use bash as preferred.
[2018年 11月 13日 星期二 15:31:44 CST] OK
[2018年 11月 13日 星期二 15:31:44 CST] Install success!

</code></pre>

### <span id="22">2.2 安装后的配置</span>

普通用户和 root 用户都可以安装使用。  
安装过程进行了以下几步:  
把 acme.sh 安装到你的 home 目录下:~/.acme.sh/并创建 一个 bash 的 alias, 方便你的使用:

<pre><code class="language-bash line-numbers">alias acme.sh=~/.acme.sh/acme.sh
echo 'alias acme.sh=~/.acme.sh/acme.sh' &gt;&gt;/etc/profile
</code></pre>

安装过程中会自动为你创建 cronjob, 每天 0:00 点自动检测所有的证书, 如果快过期了, 需要更新, 则会自动更新证书。

<pre><code class="language-bash line-numbers">00 00 * * * root /root/.acme.sh/acme.sh --cron --home /root/.acme.sh &&gt;/var/log/acme.sh.logs
</code></pre>

更高级的安装选项请参考: <https://github.com/Neilpang/acme.sh/wiki/How-to-install>  
在该脚本的安装过程不会污染已有的系统任何功能和文件, 所有的修改都限制在安装目录中: ~/.acme.sh/

## <span id="3">3.申请证书</span>

acme.sh 实现了 acme 协议支持的所有验证协议. 一般有两种方式验证: http 和 dns 验证。

### <span id="31_HTTP">3.1 HTTP 方式</span>

http 方式需要在你的网站根目录下放置一个文件, 来验证你的域名所有权,完成验证. 然后就可以生成证书了.

<pre><code class="language-bash line-numbers">acme.sh  --issue  -d clsn.io -d *.clsn.io  --webroot  /www/wwwroot/clsn.io/
</code></pre>

只需要指定域名, 并指定域名所在的网站根目录. [acme.sh][1] 会全自动的生成验证文件, 并放到网站的根目录, 然后自动完成验证. 最后会聪明的删除验证文件. 整个过程没有任何副作用.  
如果你用的 apache服务器, [acme.sh][1] 还可以智能的从 apache的配置中自动完成验证, 你不需要指定网站根目录:

<pre><code class="language-bash line-numbers">acme.sh --issue  -d clsn.io   --clsn.io
</code></pre>

如果你用的 nginx服务器, 或者反代, [acme.sh][1] 还可以智能的从 nginx的配置中自动完成验证, 你不需要指定网站根目录:

<pre><code class="language-bash line-numbers">acme.sh --issue  -d clsn.io  --nginx
</code></pre>

注意, 无论是 apache 还是 nginx 模式, acme.sh在完成验证之后, 会恢复到之前的状态, 都不会私自更改你本身的配置. 好处是你不用担心配置被搞坏。  
该类型的配置有一个缺点, 你需要自己配置 ssl 的配置, 否则只能成功生成证书, 你的网站还是无法访问https. 但是为了安全, 你还是自己手动改配置吧.  
如果你还没有运行任何 web 服务, 80 端口是空闲的, 那么 [acme.sh][1] 还能假装自己是一个webserver, 临时听在80 端口, 完成验证:

<pre><code class="language-bash line-numbers">acme.sh  --issue -d clsn.io   --standalone
</code></pre>

更高级的用法请参考: <https://github.com/Neilpang/acme.sh/wiki/How-to-issue-a-cert>

### <span id="32_DNS">3.2 DNS方式</span>

这种方式的好处是, 你不需要任何服务器, 不需要任何公网 ip, 只需要 dns 的解析记录即可完成验证。  
这种方式的缺点是，如果不同时配置 Automatic DNS API，使用这种方式 acme.sh 将无法自动更新证书，每次都需要手动再次重新解析验证域名所有权。

<span class="directory"></span>

#### <span id="321">3.2.1 颁发证书</span> {#title-0}

<pre><code class="language-bash line-numbers">[root@clsn.io /opt] 
#cd /root/.acme.sh/
[root@clsn.io /root/.acme.sh]
#acme.sh --issue -d *.clsn.io -d clsn.io --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please
[2018年 11月 13日 星期二 15:34:55 CST] Creating domain key
[2018年 11月 13日 星期二 15:34:55 CST] The domain key is here: /www/server/panel/vhost/cert/*.clsn.io/*.clsn.io.key
[2018年 11月 13日 星期二 15:34:55 CST] Multi domain='DNS:*.clsn.io,DNS:clsn.io'
[2018年 11月 13日 星期二 15:34:55 CST] Getting domain auth token for each domain
[2018年 11月 13日 星期二 15:34:56 CST] Getting webroot for domain='*.clsn.io'
[2018年 11月 13日 星期二 15:34:57 CST] Getting webroot for domain='clsn.io'
[2018年 11月 13日 星期二 15:34:57 CST] Add the following TXT record:
[2018年 11月 13日 星期二 15:34:57 CST] Domain: '_acme-challenge.clsn.io'
[2018年 11月 13日 星期二 15:34:57 CST] TXT value: '9rj0bhiGrO9UJ44XgV0APXuGBRL1vOk4XKdsnnxaIf4'
[2018年 11月 13日 星期二 15:34:57 CST] Please be aware that you prepend _acme-challenge. before your domain
[2018年 11月 13日 星期二 15:34:57 CST] so the resulting subdomain will be: _acme-challenge.clsn.io
[2018年 11月 13日 星期二 15:34:57 CST] Add the following TXT record:
[2018年 11月 13日 星期二 15:34:57 CST] Domain: '_acme-challenge.clsn.io'
[2018年 11月 13日 星期二 15:34:57 CST] TXT value: 'yJ4Ca9yVg1Fsp4RhH5XZJohh0eE3dFXEKM2KGUFNHio'
[2018年 11月 13日 星期二 15:34:57 CST] Please be aware that you prepend _acme-challenge. before your domain
[2018年 11月 13日 星期二 15:34:57 CST] so the resulting subdomain will be: _acme-challenge.clsn.io
[2018年 11月 13日 星期二 15:34:57 CST] Please add the TXT records to the domains, and re-run with --renew.
[2018年 11月 13日 星期二 15:34:57 CST] Please add '--debug' or '--log' to check more details.
[2018年 11月 13日 星期二 15:34:57 CST] See: https://github.com/Neilpang/acme.sh/wiki/How-to-debug-acme.sh

</code></pre>

<span class="directory"></span>

#### <span id="322_DNS">3.2.2 修改DNS</span> {#title-1}

在NS管理方修改主机记录

####<img data-original="https://cdn.nlark.com/yuque/0/2018/png/206952/1542094710997-aae9caf6-fce2-446d-b9cc-68a175efa6ba.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="HTTPS之acme.sh申请证书" alt="图片.png | center | 747x102" title="" /> 

3.2.3 验证解析是否生效

<pre><code class="language-bash line-numbers">$dig -t txt  _acme-challenge.clsn.io @8.8.8.8

; &lt;&lt;&gt;&gt; DiG 9.9.7-P3 &lt;&lt;&gt;&gt; -t txt _acme-challenge.clsn.io @8.8.8.8
;; global options: +cmd
;; Got answer:
;; -&gt;&gt;HEADER&lt;&lt;- opcode: QUERY, status: NOERROR, id: 35785
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;_acme-challenge.clsn.io.   IN  TXT

;; ANSWER SECTION:
_acme-challenge.clsn.io. 599    IN  TXT "yJ4Ca9yVg1Fsp4RhH5XZJohh0eE3dFXEKM2KGUFNHio"
_acme-challenge.clsn.io. 599    IN  TXT "9rj0bhiGrO9UJ44XgV0APXuGBRL1vOk4XKdsnnxaIf4"

;; Query time: 2502 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Tue Nov 13 15:40:55 CST 2018
;; MSG SIZE  rcvd: 164

</code></pre>

<span class="directory"></span>

#### <span id="324">3.2.4 生成证书</span> {#title-2}

获取Let’s Encrypt免费泛域名证书。等DNS解析生效后，运行以下命令重新生成证书：

<pre><code class="language-bash line-numbers">[root@clsn.io /root/.acme.sh]
#acme.sh --renew  -d *.clsn.io -d clsn.io --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please
[2018年 11月 13日 星期二 15:39:26 CST] Renew: '*.clsn.io'
[2018年 11月 13日 星期二 15:39:26 CST] Multi domain='DNS:*.clsn.io,DNS:clsn.io'
[2018年 11月 13日 星期二 15:39:27 CST] Getting domain auth token for each domain
[2018年 11月 13日 星期二 15:39:27 CST] Verifying:*.clsn.io
[2018年 11月 13日 星期二 15:39:29 CST] Success
[2018年 11月 13日 星期二 15:39:29 CST] Verifying:clsn.io
[2018年 11月 13日 星期二 15:39:32 CST] Success
[2018年 11月 13日 星期二 15:39:32 CST] Verify finished, start to sign.
[2018年 11月 13日 星期二 15:39:33 CST] Cert success.
-----BEGIN CERTIFICATE-----
MIIFUzCCBDugAwIBAgISBFW6y9vj0CLdCtlbjd5uRZFoMA0GCSqGSIb3DQEBCwUA
MEoxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1MZXQncyBFbmNyeXB0MSMwIQYDVQQD
ExpMZXQncyBFbmNyeXB0IEF1dGhvcml0eSBYMzAeFw0xODExMTMwNjM5MzJaFw0x
OTAyMTEwNjM5MzJaMBQxEjAQBgNVBAMMCSouY2xzbi5pbzCCASIwDQYJKoZIhvcN
AQEBBQADggEPADCCAQoCggEBANsBQz9vlST55GYdf/F9awtMqnwa2S+4nftk75NJ
s8zRLmj0LvtgZCn0fXgpNH4zAGRR3qzx7VGU80pDg539Kn8k3peoOFxRq5J3JCK2
NhvVLws7HspIsgHXngf3AyawUg+QN69K50ZDNF6/brgnOiNbmSY5fF2F9zXkNCd2
4Nsg8ptmYs3RL99ZM2CvjgLLgey3CPAuMWCse8afA4tHJ4pEZRk3dBTtWXy4q2wl
Uba8K4LuDmqOkWmQ7x3ufrq+1pQUTVQeJbKVQeDHsWQgFJVYnuHrGdd8mbz08quB
g4ruNCRnkHM1Fyxfu71IEnH1dutIeHv5OAvC3XMJUFE44kMCAwEAAaOCAmcwggJj
MA4GA1UdDwEB/wQEAwIFoDAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYBBQUHAwIw
DAYDVR0TAQH/BAIwADAdBgNVHQ4EFgQUP0ZkyUrkFWnJMVT/zGVVTfhJudgwHwYD
VR0jBBgwFoAUqEpqYwR93brm0Tm3pkVl7/Oo7KEwbwYIKwYBBQUHAQEEYzBhMC4G
CCsGAQUFBzABhiJodHRwOi8vb2NzcC5pbnQteDMubGV0c2VuY3J5cHQub3JnMC8G
CCsGAQUFBzAChiNodHRwOi8vY2VydC5pbnQteDMubGV0c2VuY3J5cHQub3JnLzAd
BgNVHREEFjAUggkqLmNsc24uaW+CB2Nsc24uaW8wTAYDVR0gBEUwQzAIBgZngQwB
AgEwNwYLKwYBBAGC3xMBAQEwKDAmBggrBgEFBQcCARYaaHR0cDovL2Nwcy5sZXRz
ZW5jcnlwdC5vcmcwggEEBgorBgEEAdZ5AgQCBIH1BIHyAPAAdgDiaUuuJujpQAno
hhu2O4PUPuf+dIj7pI8okwGd3fHb/gAAAWcMAtr/AAAEAwBHMEUCIDn0gs520um3
dbTpgm3J/QhbjCwVG7Ln8cmONkhiA0S/AiEAqV2/kS63iMF4TGOVQ9s206OPnyx8
1sLMm7NQfGrUNsYAdgApPFGWVMg5ZbqqUPxYB9S3b79Yeily3KTDDPTlRUf0eAAA
AWcMAtsEAAAEAwBHMEUCIQDqMPOXRxylye3P2SBDP/DIeDlsa8QFcWM0lYKCmYQW
zwIgGhTWGjpMOnw4fkoygDE3mHceg+k2vqUBPTwCfu+qRNIwDQYJKoZIhvcNAQEL
BQADggEBADZWQ8ms7VG10pSQVGg98VHnRE9VfgYq+zCpoJdjVv3OZZ+3hZGkxlQU
jbpQDPdx2x2KWrhPIlSylZpw1KHFjXq5pwZpkcVjkTO5tkB4+XRsUl0dENj5HGMN
pwNRPNsiWMnvxiQgKnCpIPPVTOGsajJ5ByQA4+Vni3VNvEEAOurN024e0rfzxhYV
oXBnb7tK01+HIzoOBsOl9FRbIzkmuogjCrbjU8dBudAKPNAPmFS6bwFnYeFWO2G7
DhyNrlDFJodx8JjZ9qnzD53gCfutYzsftN3jwBBiGxRpLyWXXAQLqBN5QrkTwL+h
PFn9Tul9nibZXRIQsyutHx7jFMtAyOY=
-----END CERTIFICATE-----
[2018年 11月 13日 星期二 15:39:33 CST] Your cert is in  clsn.io/clsn.io.cer
[2018年 11月 13日 星期二 15:39:33 CST] Your cert key is in  /www/server/panel/vhost/cert/clsn.io/clsn.io.key
[2018年 11月 13日 星期二 15:39:33 CST] The intermediate CA cert is in  /www/server/panel/vhost/cert/clsn.io/ca.cer
[2018年 11月 13日 星期二 15:39:33 CST] And the full chain certs is there:  /www/server/panel/vhost/cert/clsn.io/fullchain.cer
[2018年 11月 13日 星期二 15:39:33 CST] It seems that you are using dns manual mode. please take care: The dns manual mode can not renew automatically, you must issue it again manually. You'd better use the other modes instead.
[2018年 11月 13日 星期二 15:39:33 CST] Call hook error.
</code></pre>

注意第二次这里用的是 <span data-type="color" style="color:#F5222D">--renew</span>

<span class="directory"></span>

#### <span id="325_dns_API">3.2.5 dns 方式API</span> {#title-3}

dns 方式的真正强大之处在于可以使用域名解析商提供的 api 自动添加 txt 记录完成验证.  
acme.sh 目前支持 cloudflare, dnspod, cloudxns, godaddy 以及 ovh 等数十种解析商的自动集成.  
以 dnspod 为例, 你需要先登录到 dnspod 账号, 生成你的 api id 和 api key, 都是免费的. 然后:

<pre><code class="language-bash line-numbers">export DP_Id="76GJHG4SG1Q"
export DP_Key="qaEDHSJiokjhfs"
acme.sh   --issue   --dns dns_dp   -d clsn.io  -d www.clsn.io
</code></pre>

证书就会自动生成了. 这里给出的 api id 和 api key 会被自动记录下来, 将来你在使用 dnspod api 的时候, 就不需要再次指定了. 直接生成就好了:

<pre><code class="language-bash line-numbers">acme.sh  --issue   -d  clsn.io   --dns  dns_dp
</code></pre>

更详细的 api 用法: <https://github.com/Neilpang/acme.sh/blob/master/dnsapi/README.md>

## <span id="4">4.证书的使用</span>

### <span id="41">4.1 安装证书</span>

前面证书生成以后, 接下来需要把证书 copy 到真正需要用它的地方。

> 注意, 默认生成的证书都放在安装目录下: ~/.acme.sh/, 请不要直接使用此目录下的文件,  
> 例如: 不要直接让 nginx/apache 的配置文件使用这下面的文件.  
> 这里面的文件都是内部使用, 而且目录结构可能会变化.正确的使用方法是使用 --installcert 命令,并指定目标位置, 然后证书文件会被copy到相应的位置, 例如: 

<pre><code class="language-bash line-numbers">acme.sh  --installcert  -d  &lt;domain&gt;.com   \
        --key-file   /etc/nginx/ssl/&lt;domain&gt;.key \
        --fullchain-file /etc/nginx/ssl/fullchain.cer \
        --reloadcmd  "service nginx force-reload"
</code></pre>

### <span id="42_NginxTengineSSL">4.2 Nginx/Tengine服务器安装SSL证书</span>

以Nginx标准配置为例，生成的证书文件推荐使用 fullchain.cer，私钥文件为是clsn.io.key。

> Nginx 的配置 ssl&#95;certificate 使用 fullchain.cer ，而非 <domain>.cer ，否则 SSL Labs 的测试会报 Chain issues Incomplete 错误。  
> (1)通过上述中生成的证书路径为/www/server/panel/vhost/cert/clsn.io/；  
> 修改配置文件 

<pre><code class="language-bash line-numbers">    server {
        listen 443;
        server_name localhost;
        ssl on;
        root html;
        index index.html index.htm;
        ssl_certificate   /www/server/panel/vhost/cert/clsn.io/fullchain.cer;
        ssl_certificate_key  /www/server/panel/vhost/cert/clsn.io/clsn.key;
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
        location / {
            root html;
            index index.html index.htm;
        }
    }
</code></pre>

(2)重启nginx

<pre><code class="language-plain line-numbers">/etc/init.d/nginx  force-reload
</code></pre>

**注意：**这里用的是 <span data-type="color" style="color:#389E0D">service nginx force-reload</span>, 不是 service nginx reload, 据测试, reload 并不会重新加载证书, 所以用的 force-reload

## <span id="5">5.更新</span>

### <span id="51">5.1 证书的更新</span>

目前证书在 60 天以后会通过定时任务自动更新, 你无需任何操作。  
今后有可能会缩短这个时间, 不过都是自动的, 你不用关心.

### <span id="52_acmesh">5.2 acme.sh 更新</span>

目前由于 acme 协议和 letsencrypt CA 都在频繁的更新, 因此 acme.sh 也经常更新以保持同步.  
升级 acme.sh 到最新版 :

<pre><code class="language-bash line-numbers">acme.sh --upgrade
</code></pre>

如果你不想手动升级, 可以开启自动升级:

<pre><code class="language-bash line-numbers">acme.sh  --upgrade  --auto-upgrade
</code></pre>

之后, acme.sh 就会自动保持更新了.  
你也可以随时关闭自动更新:

<pre><code class="language-bash line-numbers">acme.sh --upgrade  --auto-upgrade  0
</code></pre>

### <span id="6">6.参考文献</span>

> <https://letsencrypt.org/>  
> <https://github.com/Neilpang/acme.sh/wiki/%E8%AF%B4%E6%98%8E> 

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#1lets_encryptacmesh">1.关于let's encrypt和acme.sh的简介</a><ul>
        <li>
          <a href="#11_lets_encrypt">1.1 let's encrypt</a>
        </li>
        <li>
          <a href="#12_acmesh">1.2 acme.sh</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#2acmesh">2.安装acme.sh</a><ul>
        <li>
          <a href="#21">2.1 执行安装</a>
        </li>
        <li>
          <a href="#22">2.2 安装后的配置</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#3">3.申请证书</a><ul>
        <li>
          <a href="#31_HTTP">3.1 HTTP 方式</a>
        </li>
        <li>
          <a href="#32_DNS">3.2 DNS方式</a><ul>
            <li>
              <a href="#321">3.2.1 颁发证书</a>
            </li>
            <li>
              <a href="#322_DNS">3.2.2 修改DNS</a>
            </li>
            <li>
              <a href="#324">3.2.4 生成证书</a>
            </li>
            <li>
              <a href="#325_dns_API">3.2.5 dns 方式API</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#4">4.证书的使用</a><ul>
        <li>
          <a href="#41">4.1 安装证书</a>
        </li>
        <li>
          <a href="#42_NginxTengineSSL">4.2 Nginx/Tengine服务器安装SSL证书</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#5">5.更新</a><ul>
        <li>
          <a href="#51">5.1 证书的更新</a>
        </li>
        <li>
          <a href="#52_acmesh">5.2 acme.sh 更新</a>
        </li>
        <li>
          <a href="#6">6.参考文献</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

 [1]: /wp-content/themes/clsn-003/inc/go.php?url=http://acme.sh