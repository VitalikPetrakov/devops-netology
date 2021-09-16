Домашнее задание к занятию "3.4. Операционные системы, лекция 2"
1. На лекции мы познакомились с node_exporter. В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:  
o	поместите его в автозагрузку,  
o	предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на systemctl cat cron),  
o	удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.  
Проверка автозапуска  
https://ams02pap001files.storage.live.com/y4mpg5xsbMg0nZ6RKU_ozhVpECGUcrxRdHrPgea4nKFPFQQeKeYjDIg9WvenCRoj7Kqsydf5alIafnEd9sQAMs9l_tQVDWJLsTAKIl5qDh50kpk_NLEVmnyJAob8q1s6mvAbzed2Rwl7euZktZsu7iFZ6tG6PXb6DzArvXhqfteSdeEXhIbkmYQ2TrF8tZ5n_Si?width=1291&height=472&cropmode=none  
Проверка остановки  
https://ams02pap001files.storage.live.com/y4mhAbSdI8nk3L0MvHB0ODsDGekNssDIhrrKzQY4wCNpFuO1gG_JIhbLnqYulQXxKraaSm-DkT2fl_iLRmhaQbGmRF8inYZK91Pvk6SLEa45GwWfhwLz_I6NXsh5a-W1DNGaQ1PQwf5Cg1Ziuy9j6m2VJamQTLLUNXqbX8LwKmtNorJ7OXl1DiKV8yCrQZbmsFn?width=1296&height=339&cropmode=none  
Проверка запуска   
https://ams02pap001files.storage.live.com/y4mI-OkAshyvmjIaYFvdrYi1dFumq_VauwEwMIqUB5gY3feKJKRz0amayneR-ZDoOlWtYfW_fCmXm8mxcr7eTOHOdgBrTtj8nZLQEtTb6ex3sfJJITrBGJHSXiCkUeAOqCGDVfEFyhJw-gbV3DgqrvoUSVDKAVH8TZ1BqtSM2sU22gervRVBnYroQPaLBUPHYb0?width=1311&height=367&cropmode=none  

3. Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.  

Пробросил порт 9100 на основную машину, для доппроверки автозагрузки.
https://ams02pap001files.storage.live.com/y4mv7_izdu3adQy2OEYyemU90ifvpcYcpW16cG3cJk18ltVrrN7oQPUt7URfq7kwrnYdCWKthp5e4blr2hE6PhcH-FuJFhbNkG3i0Wwq8HN6lPd9yV0XeS0CRSozBEJGzw5P5Qmw23lCSFn_p7oiklh4s6UNbVJnKo36OfRSa9YGna98OvHbfETqLIGEsLEfoQH?width=960&height=729&cropmode=none  
 
>CPU:  
•	node_cpu_seconds_total{cpu="0",mode="idle"} 75.52  
•	node_cpu_seconds_total{cpu="0",mode="system"} 5.64  
•	node_cpu_seconds_total{cpu="0",mode="user"} 1.76  
Memory:  
•	node_memory_MemAvailable_bytes  
•	node_memory_MemTotal_bytes  
Disk:  
•	node_disk_io_time_seconds_total{device="sda"}   
•	node_disk_read_bytes_total{device="sda"}   
•	node_disk_read_time_seconds_total{device="sda"}   
Network:  
•	node_network_iface_link{device="eth0"} 2  
•	node_network_info  
•	node_network_receive_bytes_total  
•	node_network_transmit_bytes_total  

	
	
3.	Установите в свою виртуальную машину Netdata. Воспользуйтесь готовыми пакетами для установки (sudo apt install -y netdata). После успешной установки:  
o	в конфигурационном файле /etc/netdata/netdata.conf в секции [web] замените значение с localhost на bind to = 0.0.0.0,  
o	добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте vagrant reload:  
>config.vm.network "forwarded_port", guest: 19999, host: 19999  
> 
После успешной перезагрузки в браузере на своем ПК (не в виртуальной машине) вы должны суметь зайти на localhost:19999. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам. 
 https://ams02pap001files.storage.live.com/y4mjNmH9NL0EpoLMvc_FhNyqApdiennGDyUEyNuDvYpvLGNd11BxCzqDj9c4va5tYRDYlBBdKyHhFPFFll720PzYFuQfooIvP2RuSfBBMcK620aYkyHyoynGlgcBMpYII8J8ClbuCX_eJ1CDvULNVEKwlPg_15NNfGUVz_pkpdZNpNBYF33XSiiaux_ZDAHD51K?width=948&height=953&cropmode=none  

4.	Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?  
>Да, можно
root@vagrant:# dmesg -H | grep virtual  
[  +0.000006] CPU MTRRs all blank - virtualized system.  
[  +0.000001] Booting paravirtualized kernel on KVM  
[  +0.000121] Performance Events: PMU not available due to virtualization, using software events only.  
[  +0.000025] systemd[1]: Detected virtualization oracle.  
5.	Как настроен sysctl fs.nr_open на системе по-умолчанию? Узнайте, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (ulimit --help)?  
>root@vagrant:#  /sbin/sysctl -n fs.nr_open  
	1048576  
    root@vagrant:#  ulimit –Sn  
    1024  

6.	Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через nsenter. Для простоты работайте в данном задании под root (sudo -i). Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.  
>root@vagrant:# ps -e | grep sleep    
   1441 pts/0    00:00:00 sleep  
root@vagrant:# nsenter --target 1441 --pid –mount  
root@vagrant:/# ps  
    PID TTY          TIME CMD  
   1493 pts/1    00:00:00 su  
   1495 pts/1    00:00:00 bash  
   1508 pts/1    00:00:00 nsenter  
   1510 pts/1    00:00:00 bash  
   1521 pts/1    00:00:00 ps  
7.	Найдите информацию о том, что такое :(){ :|:& };:. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (это важно, поведение в других ОС не проверялось). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов dmesg расскажет, какой механизм помог автоматической стабилизации. Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?  
>/etc/security/limits.conf можно задать количество лимит количества процессов для каждого юзера и не страдать от подобных вещей. А по вопросу эта конструкция плодит процессы в геометрической прогрессии до выставленного лимита (бедный мой ноут на низах заряда – долго пыжился, так как эконом режим стоял)
