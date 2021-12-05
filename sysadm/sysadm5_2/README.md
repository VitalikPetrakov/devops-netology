##Домашнее задание к занятию "5.2. Применение принципов IaaC в работе с виртуальными машинами"
________________________________________
#Задача 1  
•	Опишите своими словами основные преимущества применения на практике IaaC паттернов.  
>Основная «фишка» данного подхода – настраиваемая система автоматически разворачиваемой 
> системы. Имея файл настройки мы можем на его основе развернуть в 2 клика новую систему.    
> 
•	Какой из принципов IaaC является основополагающим?  
>При каждом разворачивании системы мы получаем один и тот же конфиг системы.  
> 
#Задача 2  
•	Чем Ansible выгодно отличается от других систем управление конфигурациями?  
>есть несколько плюсов:
>1) Легкость в изучении
>2) Написан на Python
>3) Не нужно ставить какие либо на машины - соединение идет по SSH
>4) YAML плейбуки
	
•	Какой, на ваш взгляд, метод работы систем конфигурации более надёжный push или pull?
>На мой взгляд все же push система выглядит надежней, так как есть возможно подконтрольно 
> отправлять конфигурацию под сборку, а не сама система получает файл как ей удобно. 
> 
#Задача 3  
Установить на личный компьютер:  
•	VirtualBox  
![](https://ams02pap001files.storage.live.com/y4m7_c8iy-b-Ir43FOgAwH1wso2yK3v46Qeqx6qCYncpxrlVJtAzzXT1PUyr4NuiMdz5RrgR-RsaF4zm0JsUx5ZeHADAI4fEjNMiWunhLnkcrH6PAGFU7cyp7VnAlmFm3XHjiwKX1dzVIbZbV_w9hWlG-uQFLRAnZ-xxG-E9ushFSVhMv0RcBzolBY56v0c_You?width=959&height=554&cropmode=none)
•	Vagrant  
![](https://ams02pap001files.storage.live.com/y4m6xNiavKe4h6htZSQ-kGIn4ubm6IHRquVvofKTb8gnYR2yt1qXtPu14v3OWqu0d0upf5YVGpF7NSp3vFBP0ZuISDcucD57zOiwxZoUcXTkHYuFGxG5Av6HAQ1ebG_WThO6GzUqnV-LaS0lOqxppCSSsnj_ZHMFhPMrTB9Bp-PDc9Op3X2A_BCC_61UJYFEtJj?width=803&height=360&cropmode=none)

>Vagrant и VirtualBox установлены на основной системе Windows, а Ansible получилось
установить только на WSL Ububntu, как иначе сделать я так и не понял

•	Ansible  

>root@notebook:# ansible –version  
ansible 2.9.6  
  config file = /etc/ansible/ansible.cfg  
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']  
  ansible python module location = /usr/lib/python3/dist-packages/ansible  
  executable location = /usr/bin/ansible  
  python version = 3.8.5 (default, May 27 2021, 13:30:53) [GCC 9.3.0]  

Приложить вывод команд установленных версий каждой из программ, оформленный в markdown.  

