# DNS

域名系统（Domain Name System，DNS）





根 DNS 服务器

顶级域 DNS 服务器（com）

权威 DNS 服务器（server.com）



**主服务器**：在特定区域内具有唯一性，负责维护该区域内的域名与IP地址之间的对应关系。

**从服务器**：从主服务器中获得域名与IP地址的对应关系并进行维护，以防主服务器宕机等情况。

**缓存服务器**：通过向其他域名解析服务器查询获得域名与IP地址的对应关系，并将经常查询的域名信息保存到服务器本地，以此来提高重复查询时的效率。



## BIND

BIND（Berkeley Internet Name Domain，伯克利因特网名称域）服务是全球范围内使用最广泛、最安全可靠且高效的域名解析服务程序



**主配置文件（/etc/named.conf）**：只有59行，而且在去除注释信息和空行之后，实际有效的参数仅有30行左右，这些参数用来定义bind服务程序的运行。

**区域配置文件（/etc/named.rfc1912.zones）**：用来保存域名和IP地址对应关系的所在位置。类似于图书的目录，对应着每个域和相应IP地址所在的具体位置，当需要查看或修改时，可根据这个位置找到相关文件。

**数据配置文件目录（/var/named）**：该目录用来保存域名和IP地址真实对应关系的数据配置文件。



```bash
[root@linuxprobe ~]# vim /etc/named.conf
  1 //
  2 // named.conf
  3 //
  4 // Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
  5 // server as a caching only nameserver (as a localhost DNS resolver only).
  6 //
  7 // See /usr/share/doc/bind*/sample/ for example named configuration files.
  8 //
  9 
 10 options {
 11         listen-on port 53 { any; };
 12         listen-on-v6 port 53 { ::1; };
 13         directory       "/var/named";
 14         dump-file       "/var/named/data/cache_dump.db";
 15         statistics-file "/var/named/data/named_stats.txt";
 16         memstatistics-file "/var/named/data/named_mem_stats.txt";
 17         secroots-file   "/var/named/data/named.secroots";
 18         recursing-file  "/var/named/data/named.recursing";
 19         allow-query     { any; };
 20 
 21         /* 
 22          - If you are building an AUTHORITATIVE DNS server, do NOT enable recursion.
 23          - If you are building a RECURSIVE (caching) DNS server, you need to enable 
 24            recursion. 
 25          - If your recursive DNS server has a public IP address, you MUST enable access 
 26            control to limit queries to your legitimate users. Failing to do so will
 27            cause your server to become part of large scale DNS amplification 
 28            attacks. Implementing BCP38 within your network would greatly
 29            reduce such attack surface 
 30         */
 31         recursion yes;
 32 
 33         dnssec-enable yes;
 34         dnssec-validation yes;
 35 
 36         managed-keys-directory "/var/named/dynamic";
 37 
 38         pid-file "/run/named/named.pid";
 39         session-keyfile "/run/named/session.key";
 40 
 41         /* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
 42         include "/etc/crypto-policies/back-ends/bind.config";
 43 };
 44 
 45 logging {
 46         channel default_debug {
 47                 file "data/named.run";
 48                 severity dynamic;
 49         };
 50 };
 51 
 52 zone "." IN {
 53         type hint;
 54         file "named.ca";
 55 };
 56 
 57 include "/etc/named.rfc1912.zones";
 58 include "/etc/named.root.key";
 59 
```



[https://www.linuxprobe.com/basic-learning-13.html#136]: 

