#Домашнее задание к занятию "3.3. Операционные системы, лекция 1"
1.	Какой системный вызов делает команда cd? В прошлом ДЗ мы выяснили, что cd не является самостоятельной программой, это shell builtin, поэтому запустить strace непосредственно на cd не получится. Тем не менее, вы можете запустить strace на /bin/bash -c 'cd /tmp'. В этом случае вы увидите полный список системных вызовов, которые делает сам bash при старте. Вам нужно найти тот единственный, который относится именно к cd.  
>chdir("/tmp")  
2.	Попробуйте использовать команду file на объекты разных типов на файловой системе. Например:   
>vagrant@netology1:$ file /dev/tty  
/dev/tty: character special (5/0)  
vagrant@netology1:$ file /dev/sda  
/dev/sda: block special (8/0)  
vagrant@netology1:$ file /bin/bash  
/bin/bash: ELF 64-bit LSB shared object, x86-64  
Используя strace выясните, где находится база данных file на основании которой она делает свои догадки.  
>openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libmagic.so.1", O_RDONLY|O_CLOEXEC) = 3  

3.	Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).  
>Необходимо будет узнать PID процесса, посмотреть fd этого PID через proc/PID/fd, переключиться в данный каталог cd /proc/PID/fd и через echo text > 1 перезаписать файл.  

4.	Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?  
>Зомби-процессы занимают свободные файловые дескрипторы, что опасно, так как какой нибудь плохой процесс может занять большое их колличество, что сильно замедлит систему или же вообще ее застопорит. Убить такой процесс можно только убив родительский процесс.  
	>root@vagrant:# ps -xal | grep defunct  
1     0    2597    2543  20   0      0     0 -      Z    ?          0:00 [screen] <defunct>  
0     0    3859    3666  20   0   8900   736 pipe_w S+   pts/4      0:00 grep --color=auto defunct  	
root@vagrant:# kill -9 2543  
root@vagrant:# ps -xal | grep defunct  
0     0    3899    3882  20   0   8900   740 pipe_w S+   pts/4      0:00 grep --color=auto defunct  
root@vagrant:# kill -9 3882  
Killed  
5.	В iovisor BCC есть утилита opensnoop:   
root@vagrant:# dpkg -L bpfcc-tools | grep sbin/opensnoop  
/usr/sbin/opensnoop-bpfcc  
На какие файлы вы увидели вызовы группы open за первую секунду работы утилиты? Воспользуйтесь пакетом bpfcc-tools для Ubuntu 20.04. Дополнительные сведения по установке.  
>root@vagrant:# opensnoop-bpfcc   
PID    COMM               FD ERR PATH  
797    bash                3   0 /proc/uptime  
797    bash                3   0 /var/cache/netdata/.netdata_bash_sleep_timer_fifo  
878    apps.plugin         4   0 /proc/1/stat  
878    apps.plugin         5   0 /proc/1/io  
878    apps.plugin         6   0 /proc/1/status  
878    apps.plugin         7   0 /proc/1/fd  
878    apps.plugin         4   0 /proc/2/stat  
878    apps.plugin         5   0 /proc/2/io  
878    apps.plugin         6   0 /proc/2/status  
6.	Какой системный вызов использует uname -a? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в /proc, где можно узнать версию ядра и релиз ОС.  
>Part of the utsname information is also accessible  via  /proc/sys/ker‐
       nel/{ostype, hostname, osrelease, version, domainname}    
uname({sysname="Linux", nodename="vagrant", ...}) = 0  

7.	Чем отличается последовательность команд через ; и через && в bash? Например:   
root@netology1:# test -d /tmp/some_dir; echo Hi  
Hi  
root@netology1:# test -d /tmp/some_dir && echo Hi  
root@netology1:#  
Есть ли смысл использовать в bash &&, если применить set -e?  
>При использовании символа ; команды исполняются последовательно, не смотря на результат предыдущей, при использовании && - вторая команда выполниться только тогда, когда первая завершилась без ошибки. Но, если добавить в первую команду set –e то не будет разницы между ; и &&, так как при ошибке команда просто отбросится, по аналогии с continue в питоне.   
8.	Из каких опций состоит режим bash set -euxo pipefail и почему его хорошо было бы использовать в сценариях?  
>-e: скрипт немедленно завершит работу, если любая команда выйдет с ошибкой  
-u: прерывает скрипт если не указаны все переменные  
-x: отображение каждой строки скрипта  
-o: прерывает скрипт, если любая команда, подключенная через | выполняется с ошибкой
9.	Используя -o stat для ps, определите, какой наиболее часто встречающийся статус у процессов в системе. В man ps ознакомьтесь (/PROCESS STATE CODES) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).  
>root@vagrant:# ps -o stat  
STAT  
S    
R+  
D  
additional characters may be displayed:  
               <    high-priority (not nice to other users)  
               N    low-priority (nice to other users)  
               L    has pages locked into memory (for real-time and custom IO)  
               s    is a session leader  
               l    is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)  
               +    is in the foreground process group  



