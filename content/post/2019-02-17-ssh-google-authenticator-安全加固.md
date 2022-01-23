---
title: SSH + Google Authenticator 安全加固
author: 惨绿少年
type: post
date: 2019-02-17T02:11:10+00:00
url: /clsn/lx1502.html
Baidusubmit:
  - 1
categories:
  - 云计算
  - 安全
  - 运维基本功

---
<div id="log-box">
  <div id="catalog">
    <ul id="catalog-ul">
      <li>
        <i class="be be-arrowright"></i> <a href="#title-0" title="3.2.1 安装依赖包">3.2.1 安装依赖包</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-1" title="3.2.2 Google Authenticator PAM插件安装">3.2.2 Google Authenticator PAM插件安装</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-2" title="3.2.3 复制so文件">3.2.3 复制so文件</a>
      </li>
    </ul>
    
    <span class="log-zd"><span class="log-close"><a title="隐藏目录"><i class="be be-cross"></i><strong>目录</strong></a></span></span>
  </div>
</div>

# <span id="SSH_Google_Authenticator">SSH + Google Authenticator 安全加固</span>

## <span id="1_SSH">1. SSH连接</span>

<img data-original="https://cdn.nlark.com/yuque/0/2018/png/206952/1543929389178-51ab7b60-4303-4cb5-8083-88794e6b6599.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="SSH + Google Authenticator 安全加固" alt="image.png | center | 320x233" title="" />  
Secure Shell（安全外壳协议，简称SSH）是一种加密的网络传输协议，可在不安全的网络中为网络服务提供安全的传输环境。SSH通过在网络中创建安全隧道来实现SSH客户端与服务器之间的连接。虽然任何网络服务都可以通过SSH实现安全传输，SSH最常见的用途是远程登录系统，人们通常利用SSH来传输命令行界面和远程执行命令。使用频率最高的场合类Unix系统，但是Windows操作系统也能有限度地使用SSH。  
<span data-type="color" style="color:rgb(85, 85, 85)"><span data-type="background" style="background-color:rgb(255, 255, 255)">SSH本身是一个非常安全的认证连接方式。不过由于人过等方面的原因，难免会造成密码的泄露。针对这种问题我们不妨给SSH再加一把锁。当然，增加这层锁的方式有很多种。例如：knockd、S/KEY、OPIE/OPTW、Two-factor authentication等。</span></span>

## <span id="2_Google_Authenticator">2. Google Authenticator</span>

<img data-original="https://cdn.nlark.com/yuque/0/2018/png/206952/1543927721094-81e9c2a8-ec0b-4a64-b546-b2feb1023ba1.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="SSH + Google Authenticator 安全加固" alt="image.png | center | 559.746835443038x180" title="" /> 

Google身份验证器是一款基于时间与哈希的一次性密码算法的两步验证软件令牌，此软件用于Google的认证服务。此项服务所使用的算法已列于 RFC 6238 和 RFC 4226 中。   
Google身份验证器给予用户一个六位到八位的一次性密码用于进行登录Google或其他站点时的附加验证。其同样可以给第三方应用生成口令，例如密码管家程序或网络硬盘。先前版本的Google身份验证器开放源代码，但之后的版本以专有软件的形式公开。

## <span id="3Linux">3.Linux 中安装</span>

### <span id="31">3.1 系统环境说明</span>

<pre><code class="language-bash line-numbers">[root@clsn.io /root] clsn.io Blog WebSite
#cat  /etc/redhat-release 
CentOS release 6.8 (Final)

[root@clsn.io /root] clsn.io Blog WebSite
#uname -a
Linux clsn.io 4.10.5-1.el6.elrepo.x86_64 #1 SMP Wed Mar 22 14:55:33 EDT 2017 x86_64 x86_64 x86_64 GNU/Linux

[root@clsn.io /root] clsn.io Blog WebSite
#sestatus 
SELinux status:                 disabled
</code></pre>

### <span id="32_Google_Authenticator">3.2 安装 Google Authenticator</span>

<span class="directory"></span>

#### <span id="321">3.2.1 安装依赖包</span> {#title-0}

<pre><code class="language-bash line-numbers">yum -y install wget gcc make pam-devel libpng-devel
</code></pre>

<span class="directory"></span>

#### <span id="322_Google_Authenticator_PAM">3.2.2 Google Authenticator PAM插件安装</span> {#title-1}

<pre><code class="language-bash line-numbers"># 可在google的github下载
wget  https://github.com/google/google-authenticator/archive/1.02.tar.gz 
tar xf 1.02.tar.gz
cd google-authenticator-1.02/libpam/
./bootstrap.sh
./configure 
make && make install
</code></pre>

安装完成后会在 /usr/local/lib/security/pam&#95;google&#95;authenticator.so生成一个 库文件，  
系统还会多在/usr/local/bin目录生成一个google－authenticator可执行文件，通过运行该命令进行配置。

<span class="directory"></span>

#### <span id="323_so">3.2.3 复制so文件</span> {#title-2}

<pre><code class="language-bash line-numbers"># cp  /usr/local/lib/security/pam_google_authenticator.so /lib64/security/
</code></pre>

## <span id="4_SSH_Google_Authenticator">4. 配置 SSH + Google Authenticator</span>

### <span id="41_Google_Authenticator">4.1 初始配置 Google Authenticator</span>

<pre><code class="language-bash line-numbers">[root@clsn.io /lib64/security] clsn.io Blog WebSite
#google-authenticator 
Do you want authentication tokens to be time-based (y/n) n
# 是否基于时间的认证，为了防止不同跨时区的问题，这里选择n
https://www.google.com/chart?chs=200x200&chld=M|0&cht=qr&chl=otpauth://hotp/root@clsn.io%3Fsecret%3*****%26issuer%3Dclsn.io
# s生成的二维码
Your new secret key is: ****
Your verification code is 5****0
Your emergency scratch codes are:
  40****84
  19****95
  60****78
  83****92
  31****58
# 这5个码用于在取不到或错的验证码有错时，用于应急用的。不过每个只能用一次，不能重复使用。

Do you want me to update your "/root/.google_authenticator" file? (y/n) y

By default, three tokens are valid at any one time.  This accounts for
generated-but-not-used tokens and failed login attempts. In order to
decrease the likelihood of synchronization problems, this window can be
increased from its default size of 3 to 17. Do you want to do so (y/n) y

If the computer that you are logging into isn't hardened against brute-force
login attempts, you can enable rate-limiting for the authentication module.
By default, this limits attackers to no more than 3 login attempts every 30s.
Do you want to enable rate-limiting (y/n) y


</code></pre>

### <span id="42_SSH">4.2 SSH调用及客户端配置</span>

添加pam认证，<span data-type="color" style="color:rgb(79, 79, 79)"><span data-type="background" style="background-color:rgb(255, 255, 255)">在第一行添加 </span></span>

<pre><code class="language-bash line-numbers"># vim  /etc/pam.d/sshd
auth       required     pam_google_authenticator.so
------------------------------------------------------------------
#cat  /etc/pam.d/sshd 
#%PAM-1.0
auth       required pam_sepermit.so
auth       required     pam_google_authenticator.so
auth       include      password-auth
account    required     pam_nologin.so
account    include      password-auth
password   include      password-auth
# pam_selinux.so close should be the first session rule
session    required     pam_selinux.so close
session    required     pam_loginuid.so
# pam_selinux.so open should only be followed by sessions to be executed in the user context
session    required     pam_selinux.so open env_params
session    required     pam_namespace.so
session    optional     pam_keyinit.so force revoke
session    include      password-auth

</code></pre>

修改sshd配置

<pre><code class="language-bash line-numbers"># vim /etc/ssh/sshd_config
ChallengeResponseAuthentication no
#把上面配置改成
</code></pre>

重启 sshd 服务

<pre><code class="language-plain line-numbers"># service sshd restart 
</code></pre>

## <span id="5">5. 客户端使用</span>

### <span id="51_Android">5.1 Android客户端</span>

(版本<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">5.00，更新日期 2017年9月27日</span></span>)  
下载地址：<https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2&hl=zh>  
CLSN镜像地址 <https://clsn.io/files/google/com.google.android.apps.authenticator.apk>

<img data-original="https://cdn.nlark.com/yuque/0/2018/png/206952/1543931686973-acb91a8a-66cd-4a65-b0d3-26cfa16cc259.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="SSH + Google Authenticator 安全加固" alt="image.png | center | 189.4736842105263x400" title="" /> 

### <span id="62">6.2 浏览器客户端</span>

获取30秒一次的动态码的客户端是浏览器（仅支持chrome、firefox）、Android设备、苹果IOS设备、Blackberry、WP手持设备。各自程序的下载地址为：  
[chrome google-authenticator插件][1]  
[firefox google-authenticator插件][2] 

### <span id="63_Python">6.3 Python 客户端</span>

<pre><code class="language-python line-numbers">import hmac, base64, struct, hashlib, time

def calGoogleCode(secretKey):
    input = int(time.time())//30
    key = base64.b32decode(secretKey)
    msg = struct.pack("&gt;Q", input)
    googleCode = hmac.new(key, msg, hashlib.sha1).digest()
    o = ord(googleCode[19]) & 15
    googleCode = str((struct.unpack("&gt;I", googleCode[o:o+4])[0] & 0x7fffffff) % 1000000)
    if len(googleCode) == 5:
        googleCode = '0' + googleCode
    return googleCode

secretKey = '***这里填秘钥***'
print calGoogleCode(secretKey)
</code></pre>

## <span id="6">6. 参考文献</span>

> [http://www.361way.com/google-authenticator-ssh/2186.html][3]  
> [http://netsecurity.51cto.com/art/201305/392443.htm][4]  
> <https://blog.csdn.net/bwlab/article/details/51321746>  
> <https://github.com/google/google-authenticator/wiki>  
> <https://blog.csdn.net/RBPicsdn/article/details/81155054> 

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#SSH_Google_Authenticator">SSH + Google Authenticator 安全加固</a><ul>
        <li>
          <a href="#1_SSH">1. SSH连接</a>
        </li>
        <li>
          <a href="#2_Google_Authenticator">2. Google Authenticator</a>
        </li>
        <li>
          <a href="#3Linux">3.Linux 中安装</a><ul>
            <li>
              <a href="#31">3.1 系统环境说明</a>
            </li>
            <li>
              <a href="#32_Google_Authenticator">3.2 安装 Google Authenticator</a><ul>
                <li>
                  <a href="#321">3.2.1 安装依赖包</a>
                </li>
                <li>
                  <a href="#322_Google_Authenticator_PAM">3.2.2 Google Authenticator PAM插件安装</a>
                </li>
                <li>
                  <a href="#323_so">3.2.3 复制so文件</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#4_SSH_Google_Authenticator">4. 配置 SSH + Google Authenticator</a><ul>
            <li>
              <a href="#41_Google_Authenticator">4.1 初始配置 Google Authenticator</a>
            </li>
            <li>
              <a href="#42_SSH">4.2 SSH调用及客户端配置</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#5">5. 客户端使用</a><ul>
            <li>
              <a href="#51_Android">5.1 Android客户端</a>
            </li>
            <li>
              <a href="#62">6.2 浏览器客户端</a>
            </li>
            <li>
              <a href="#63_Python">6.3 Python 客户端</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#6">6. 参考文献</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

 [1]: https://chrome.google.com/webstore/detail/gauth-authenticator/ilgcnhelpchnceeipipijaljkblbcobl?utm_source=chrome-ntp-icon
 [2]: https://marketplace.firefox.com/app/gauth-authenticator/
 [3]: /wp-content/themes/clsn-003/inc/go.php?url=http://www.361way.com/google-authenticator-ssh/2186.html
 [4]: /wp-content/themes/clsn-003/inc/go.php?url=http://netsecurity.51cto.com/art/201305/392443.htm