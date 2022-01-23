---
title: CLSN Whois API
author: 惨绿少年
type: post
date: 2019-01-30T08:20:38+00:00
url: /clsn/lx1494.html
Baidusubmit:
  - 1
categories:
  - 运维基本功

---
<img data-original="https://cdn.nlark.com/yuque/0/2019/png/206952/1548836299574-86cd55fa-9d67-4214-a346-8bfa5f28b096.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="CLSN Whois API" alt="" />

## <span id="API">API地址</span>

CLSN whois API 地址：https://whois.clsn.io/

## <span id="API-2">API使用方法</span>

<pre><code class="language-shell line-numbers">curl https://whois.clsn.io/?domain=*domain*
</code></pre>

例如

<pre><code class="language-shell line-numbers">$ curl https://whois.clsn.io/?domain=clsn.io

Welcome clsn domain whois:
Domain Name: CLSN.IO
Registry Domain ID: D503300000141403578-LRMS
Registrar WHOIS Server: whois.gandi.net
Registrar URL: https://www.gandi.net/whois
Updated Date: 2018-11-17T20:31:59Z
Creation Date: 2018-09-18T06:09:13Z
Registry Expiry Date: 2019-09-18T06:09:13Z
Registrar Registration Expiration Date:
Registrar: Gandi SAS
Registrar IANA ID: 81
Registrar Abuse Contact Email: abuse@support.gandi.net
Registrar Abuse Contact Phone: +33.170377661
Reseller:
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Registrant Organization:
Registrant State/Province:
Registrant Country: CN
Name Server: NS1.ALIDNS.COM
Name Server: NS2.ALIDNS.COM
Name Server: NS3.ALIDNS.COM
Name Server: NS4.ALIDNS.COM
DNSSEC: unsigned

&gt;&gt;&gt; Last update of WHOIS database: 2019-01-30T08:14:01Z &lt;&lt;&lt;

For more information on Whois status codes, please visit https://icann.org/epp

Access to WHOIS information provided by Internet Computer Bureau Ltd. ("ICB") is provided to assist persons in determining the contents of a domain name registration record in the ICB registry database. The data in this record is provided by ICB for informational purposes only, and ICB does not guarantee its accuracy. This service is intended only for query-based access. You agree that you will use this data only for lawful purposes and that, under no circumstances will you use this data to(i) allow, enable, or otherwise support the transmission by e-mail, telephone, facsimile or other electronic means of mass, unsolicited, commercial advertising or solicitations to entities other than the data recipient's own existing customers; or (ii) enable high volume, automated, electronic processes that send queries or data to the systems of Registry Operator, a Registrar, or ICB or its services providers except as reasonably necessary to register domain names or modify existing registrations. UK privacy laws limit the scope of information permitted for certain public access.  Therefore, concerns regarding abusive use of domain registrations in the ICB registry should be directed to either (a) the Registrar of Record as indicated in the WHOIS output, or (b) the ICB anti-abuse department at abuse@icbregistry.info.

All rights reserved. ICB reserves the right to modify these terms at any time. By submitting this query, you agree to abide by these policies

The Registrar of Record identified in this output may have an RDDS service that can be queried for additional information on how to contact the Registrant, Admin, or Tech contact of the queried domain name.
</code></pre>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#API">API地址</a>
    </li>
    <li>
      <a href="#API-2">API使用方法</a>
    </li>
  </ul>
</div>