Домашнее задание к занятию "3.1. Работа в терминале, лекция 1"

4.	С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:
o	Создайте директорию, в которой будут храниться конфигурационные файлы Vagrant. В ней выполните vagrant init. Замените содержимое Vagrantfile по умолчанию следующим:
o	 Vagrant.configure("2") do |config|
o	 	config.vm.box = "bento/ubuntu-20.04"
end

PS C:\devops\devops-netology\sysadm\vagrant> ls  


    Каталог: C:\devops\devops-netology\sysadm\vagrant  

Mode                 LastWriteTime         Length Name  

-a----        02.08.2021     18:11           3080 Vagrantfile  
	
Выполнение в этой директории vagrant up установит провайдер VirtualBox для Vagrant, скачает необходимый образ и запустит виртуальную машину.

PS C:\devops\devops-netology\sysadm\vagrant> vagrant up --color –timestamp  
Bringing machine 'default' up with 'virtualbox' provider...  
==> default: Box 'bento/ubuntu-20.04' could not be found. Attempting to find and install...  
    default: Box Provider: virtualbox  
    default: Box Version: >= 0  
==> default: Loading metadata for box 'bento/ubuntu-20.04'  
    default: URL: https://vagrantcloud.com/bento/ubuntu-20.04  
==> default: Adding box 'bento/ubuntu-20.04' (v202107.28.0) for provider:   virtualbox  
    default: Downloading: https://vagrantcloud.com/bento/boxes/ubuntu-20.04/versions/202107.28.0/providers/virtualbox.box  

vagrant suspend выключит виртуальную машину с сохранением ее состояния (т.е., при следующем vagrant up будут запущены все процессы внутри, которые работали на момент вызова suspend), vagrant halt выключит виртуальную машину штатным образом.
5.	Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию
      
 1gb оперативной памяти, 2 ядра и 64gb жесткий диск
6.	Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?
      
Изменение стандартных значений делается в Vagrantfile   
v.customize ["modifyvm", :id, "--memory", [ENV['DISCOURSE_VM_MEM'].to_i, 1024].max]  
cpu_count = 2  

7.	Команда vagrant ssh из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.
8.	Ознакомиться с разделами man bash, почитать о настройках самого bash:
В	какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?  
      
Строка 982  
HISTSIZE  
              The number of commands to remember in the command history (see  HISTORY  below).   If  the value is 0, commands are not saved in the history list.  Numeric values less tha zero result in every command being saved on the history list  (there  is  no  limit). The shell sets the default value to 500 after reading any startup files
o	что делает директива ignoreboth в bash?
A value of ignoreboth is shorthand for ignorespace and ignoredups  
9.	В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?

Не понял вопроса, фигурные скорбки это способ задания списка, а список может быть использован в многих случаях. Либо я не понял что именно нужно искать. 
10.	Основываясь на предыдущем вопросе, как создать однократным вызовом touch 100000 файлов? А получилось ли создать 300000?  

Touch test{1..100000}  
root@vagrant:~/test# ls -l | wc  
 100001  900002 4788903  
root@vagrant:~/test# touch test{1..300000}  
-bash: /usr/bin/touch: Argument list too long  
11.	В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]  

Возвращает значение 1 или 0 в зависимости от того, верно ли значение в квадратных скобках. То есть [[ -d /tmp ]] вернет 1 так как /tmp это папка  
12.	Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:  

bash is /tmp/new_path_directory/bash  
bash is /usr/local/bin/bash  
bash is /bin/bash  
(прочие строки могут отличаться содержимым и порядком)  
root@vagrant:~# type -a bash  
bash is /usr/bin/bash  
bash is /bin/bash  
root@vagrant:~# mkdir /tmp/new_bash  
root@vagrant:~# cp /bin/bash /tmp/new_bash/  
root@vagrant:~# PATH=/tmp/new_bash/:$PATH  
root@vagrant:~# type -a bash  
bash is /tmp/new_bash/bash  
bash is /usr/bin/bash  
bash is /bin/bash  
13.	Чем отличается планирование команд с помощью batch и at?  
At – планировщик задач в заданное время но только 1 раз.    
Batch – пакетный планировщик задач, который работает основываясь на загрузку системы, если система загружена больше чем указано то данный планировщик ожидает уменьшения загрузки.  
14.	Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.  
vagrant suspend  


