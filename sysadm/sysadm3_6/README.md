#Домашнее задание к занятию "3.6. Компьютерные сети, лекция 1"
1.	Работа c HTTP через телнет.  
•	Подключитесь утилитой телнет к сайту stackoverflow.com telnet stackoverflow.com 80  
•	отправьте HTTP запрос  
GET /questions HTTP/1.0  
HOST: stackoverflow.com  
[press enter]  
[press enter]  
•	В ответе укажите полученный HTTP код, что он означает?  
>root@notebook:/mnt/c/devops/devops-netology# telnet stackoverflow.com 80  
Trying 151.101.65.69...    
Connected to stackoverflow.com.  
Escape character is '^]'.  
GET /questions HTTP/1.0  
HOST: stackoverflow.com  
HTTP/1.1 301 Moved Permanently  
cache-control: no-cache, no-store, must-revalidate    
location: https://stackoverflow.com/questions  
x-request-guid: 9c5ced67-0ecd-442d-9f4c-0cc5942f2d88  
feature-policy: microphone 'none'; speaker 'none'  
content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com  
Accept-Ranges: bytes  
Date: Thu, 09 Sep 2021 16:13:29 GMT  
Via: 1.1 varnish  
Connection: close  
X-Served-By: cache-ams21030-AMS  
X-Cache: MISS  
X-Cache-Hits: 0  
X-Timer: S1631204010.634264,VS0,VE75  
Vary: Fastly-SSL  
X-DNS-Prefetch-Control: off  
Set-Cookie: prov=c0be3600-58ca-d2ba-52c7-6ce28faaab54; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly   
Connection closed by foreign host.  
>Судя по ответу 301 – сайт перехал, но так как вебверсия работает, но уже на HTTPS, то редерект на шифрованый канал telnet не перекидывает.  

3. Повторите задание 1 в браузере, используя консоль разработчика F12.  
•	откройте вкладку Network  
•	отправьте запрос http://stackoverflow.com  
•	найдите первый ответ HTTP сервера, откройте вкладку Headers  
•	укажите в ответе полученный HTTP код.  
>HTTP/2 200 OK   
cache-control: private  
content-type: text/html; charset=utf-8  
content-encoding: gzip  
strict-transport-security: max-age=15552000  
x-frame-options: SAMEORIGIN  
x-request-guid: 6048f339-68b6-4172-8aa0-470023362cf6  
feature-policy: microphone 'none'; speaker 'none'  
content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com  
accept-ranges: bytes 
date: Thu, 09 Sep 2021 16:19:33 GMT  
via: 1.1 varnish  
x-served-by: cache-ams21083-AMS  
x-cache: MISS  
x-cache-hits: 0  
x-timer: S1631204374.716647,VS0,VE78  
vary: Accept-Encoding,Fastly-SSL  
x-dns-prefetch-control: off  
X-Firefox-Spdy: h2  
•	проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?  
Судя по таймингам, самый долгий запрос был именно первый HTTP запрос (код выше)  
 ![](https://ams02pap001files.storage.live.com/y4mK1b4t1OHg7jtYQC48AR5LSlAish9MZ6sOYISzl4FsxuCsVbaO201mgQOPQduiskW7xGSqMySiAZsNQxApIzU0Y4bK57S0oTYSH6pUx-bQyGS6Ayvwz71PoQPd0R643wFKJWDHAKu1sQgPFMliZBADuK42alUs5zlLDslhGkuftJO8sNIP1XA5KShEbrDJ5bg?width=543&height=223&cropmode=none)  
•	приложите скриншот консоли браузера в ответ.  
 ![](https://ams02pap001files.storage.live.com/y4mp1odeKo4FjVw2D2iaR_wimjkb_Do7FS_DMOr_dBGEo7vER3_T82wdd7v3Ku0MtLt8L-M_6u8MPq-CvIWEBHvXFYi61PvfhB_taUFneSnX-s0B2SOC6fPJ1JmBvP_eN8M7B-KEVg-jtw9TrjZi7R6ZOIJyJovj9h_VINNFBmRPNICHJpFgqEdA2wNYVvyPoaX?width=1919&height=998&cropmode=none)  
3.	Какой IP адрес у вас в интернете?  
 ![](https://ams02pap001files.storage.live.com/y4mG5r7p62zHEoJ4-QPPmJYDBdkAZLsFka6gCM6ubEV_INDBbZtbKnn0QhvLPgYm-L7H46pVK5XWieu-FdFg8RctQ41baxJg2iz8cCYZvR8xVKQ8cgLct9S-h0zYSS-VwpTAAaocuw2rxXxVPh2k586YZoVUyPWjJJcxmFJxFY3fYlP28V7pOrSdoKjt4O2h5bh?width=579&height=165&cropmode=none)  
4.	Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой whois  
>org-name:       OOO WestCall Ltd.  
root@notebook:/mnt/c/devops/devops-netology# whois -h whois.radb.net 217.172.29.117  
route:          217.172.28.0/22  
origin:         AS8595  
mnt-by:         WESTCALL-MNT  
created:        2017-07-07T18:45:09Z  
last-modified:  2017-07-07T18:45:09Z  
5.	Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой traceroute  
 ![](https://ams02pap001files.storage.live.com/y4mjNBOREkTV7q4C8Xl8WgFBUn9BfIy7oeS-TEOuZrWmEylQOrguEml_lV5lu-iRv2CEktHRy8RlMdVGivnPtATtCVgCD0S8YoRqRQqMopGfecRTRnvm0rK1TE8r6b2Q611VI8t-EbZp6MDQmEyeNIgETM5ZTMvqb_Mi1RQagPQJ0W4i78pvb66jkUfhTCxfhgF?width=950&height=493&cropmode=none)  
6.	Повторите задание 5 в утилите mtr. На каком участке наибольшая задержка - delay?  
>В моем случае 217.172.29.113  
 ![](https://ams02pap001files.storage.live.com/y4mW2m3jP1TV7eLkzQZBunSSxRx5k2zjVqtIoWNxNQsRjbB-uSjBFosnUcL2eV0bZCulWTTTYy9oOral3Unci2tmZg9iTD9USeaTdor-gJlMX_oh-lZYJLLZnJuSC55FsgZhff8XRislmFVqOs-DpUoLwZtwyGbYG25T-PXSNOZubB0CugC2PkuiZxY1k6MX_zS?width=943&height=587&cropmode=none)  
7.	Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь утилитой dig 
>dns.google.             448     IN      A       8.8.8.8  
dns.google.             448     IN      A       8.8.4.4  
dns.google.             600     IN      NS      ns1.zdns.google.  
dns.google.             600     IN      NS      ns4.zdns.google.  
dns.google.             600     IN      NS      ns2.zdns.google.  
dns.google.             600     IN      NS      ns3.zdns.google.  
8.	Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь утилитой dig  
>4.4.8.8.in-addr.arpa.   441     IN      PTR     dns.google.  

