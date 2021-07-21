# devops-netology
1.	Склонируйте репозиторий, используя https протокол (git clone ...) 

/devops$ sudo git clone https://github.com/VitalikPetrakov/devops-netology.git  
[sudo] password for user:  
Cloning into 'devops-netology'...  
remote: Enumerating objects: 3, done.  
remote: Counting objects: 100% (3/3), done.  
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0  
Unpacking objects: 100% (3/3), 601 bytes | 8.00 KiB/s, done.  

2.	Перейдите в каталог с клоном репозитория (cd devops-netology) 

/devops$ cd devops-netology/  
devops-netology$  

3.	Произведите первоначальную настройку git, указав свое настоящее имя (пожалуста, используйте настоящие имена, нам так будет проще общаться) и email (git config --global user.name и git config --global user.email johndoe@example.com). 

devops-netology# git config -l  
user.name=VitalyPetrakov  
user.email=vitalik.petrakov@gmail.com  
core.repositoryformatversion=0  
core.filemode=false  
core.bare=false  
core.logallrefupdates=true  
core.ignorecase=true  
remote.origin.url=https://github.com/VitalikPetrakov/devops-netology.git  
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*  
branch.main.remote=origin    
branch.main.merge=refs/heads/main  

4.	Выполните команду git status и запомните результат. 

devops-netology# git status  
On branch main  
Your branch is up to date with 'origin/main'.  

nothing to commit, working tree clean  

5.	Отредактируйте файл README.md любым удобным способом, тем самым переведя файл в состояние Modified. 

devops-netology# echo "1 line" > README.md  
devops-netology# git status  
On branch main  
Your branch is up to date with 'origin/main'.  
  
Changes not staged for commit:  
  (use "git add <file>..." to update what will be committed)  
  (use "git restore <file>..." to discard changes in working directory)  
        modified:   README.md  
  
no changes added to commit (use "git add" and/or "git commit -a")  

6.	Еще раз выполните git status и продолжайте проверять вывод этой команды после каждого последующего шага. 
7.	Давйте теперь посмотрим изменения в файле README.md выполнив команды git diff и git diff --staged. 

devops-netology# git diff  
diff --git a/README.md b/README.md  
index 647b370..7c28d47 100644  
--- a/README.md  
+++ b/README.md  
@@ -1 +1 @@  
-# devops-netology  
\ No newline at end of file  
+1 line  

devops-netology# git diff --staged  
devops-netology#  

devops-netology# git status  
On branch main  
Your branch is up to date with 'origin/main'.  

Changes not staged for commit:  
  (use "git add <file>..." to update what will be committed)  
  (use "git restore <file>..." to discard changes in working directory)  
        modified:   README.md  

no changes added to commit (use "git add" and/or "git commit -a")  


8.	Переведите файл в состояние staged (или как говорят просто добавьте файл в коммит) командой git add README.md. 

devops-netology# git add README.md  
devops-netology# git status  
On branch main  
Your branch is up to date with 'origin/main'.  

Changes to be committed:  
  (use "git restore --staged <file>..." to unstage)  
        modified:   README.md  

9.	И еще раз выполните команды git diff и git diff --staged. Поиграйте с изменениями и этими коммандами, чтобы четко понять что и когда они отображают. 

devops-netology# git diff  
devops-netology# git diff --staged  
diff --git a/README.md b/README.md  
index 647b370..7c28d47 100644  
--- a/README.md  
+++ b/README.md  
@@ -1 +1 @@  
-# devops-netology  
\ No newline at end of file  
+1 line  

10.	Теперь можно сделать коммит git commit -m 'First commit'. 
 
devops-netology# git commit -m "First commit"  
[main 64bf7cd] First commit  
 1 file changed, 1 insertion(+), 1 deletion(-)   

11.	И еще раз посмотреть выводы команд git status, git diff и git diff --staged.
devops-netology# git status  
On branch main  
Your branch is ahead of 'origin/main' by 1 commit.  
  (use "git push" to publish your local commits)  

nothing to commit, working tree clean  
devops-netology# git diff  
devops-netology# git diff –staged  

Создадим файлы .gitignore и второй коммит:

1.	Создайте файл .gitignore (обратите внимание на точку в начале файла), проверьте его статус сразу после создания.
devops-netology# touch .gitignore   
devops-netology# ls -l  
total 0  
-rwxrwxrwx 1 root root 7 Jul 21 15:12 README.md  
devops-netology# ls -lha  
total 0  
drwxrwxrwx 1 root root 4.0K Jul 21 15:18 .  
drwxrwxrwx 1 root root 4.0K Jul 21 14:54 ..  
drwxrwxrwx 1 root root 4.0K Jul 21 15:17 .git  
-rwxrwxrwx 1 root root    0 Jul 21 15:18 .gitignore  
-rwxrwxrwx 1 root root    7 Jul 21 15:12 README.md   
2.	Добавьте файл .gitignore в следующий коммит (git add...).  
devops-netology# git add .gitignore  
devops-netology# git status   
On branch main  
Your branch is ahead of 'origin/main' by 1 commit.  
  (use "git push" to publish your local commits)  

Changes to be committed:  
  (use "git restore --staged <file>..." to unstage)  
        new file:   .gitignore  
3.	На одном из следующих блоков мы будем изучать Terraform, давайте сразу же создадим соотвествующий каталог terraform и внутри этого каталога файл .gitignore по этому примеру: https://github.com/github/gitignore/blob/master/Terraform.gitignore.
devops-netology# mkdir Terraform  
devops-netology# cd Terraform/  
devops-netology/Terraform# touch .gitignore  
devops-netology/Terraform# nano .gitignore  
devops-netology/Terraform# ls -la  
total 4  
drwxrwxrwx 1 root root 4096 Jul 21 15:21 .  
drwxrwxrwx 1 root root 4096 Jul 21 15:20 ..  
-rwxrwxrwx 1 root root  862 Jul 21 15:21 .gitignore  
4.	В файле README.md опишите своими словами какие файлы будут проигнорированы в будущем благодаря добавленному .gitignore.
Файлы, помеченный точкой как невидимые для системы будут игнорироватся при пушах и коммитах проекта, если не добавить вручную через , но гит при это знает о них.
5.	Закоммитте все новые и измененные файлы. Комментарий к коммиту должен быть Added gitignore.  
devops-netology# git add Terraform/  
devops-netology# git status  
On branch main  
Your branch is ahead of 'origin/main' by 1 commit.  
  (use "git push" to publish your local commits)  
  
Changes to be committed:  
  (use "git restore --staged <file>..." to unstage)  
        new file:   .gitignore  
        new file:   Terraform/.gitignore  

devops-netology# git commit -m "Added gitignore"  
[main 8b7f83f] Added gitignore  
 2 files changed, 34 insertions(+)  
 create mode 100644 .gitignore  
 create mode 100644 Terraform/.gitignore  

Экспериментируем с удалением и перемещением файлов (третий и четвертый коммит).  

1.	Создайте файлы will_be_deleted.txt (с текстом will_be_deleted) и will_be_moved.txt (с текстом will_be_moved) и закоммите их с комментарием Prepare to delete and move.
devops-netology# echo "will_be_deleted" > will_be_deleted.txt   
devops-netology# echo "will_be_moved" > will_be_moved.txt  
devops-netology# ls -l  
total 0  
-rwxrwxrwx 1 root root    7 Jul 21 15:12 README.md  
drwxrwxrwx 1 root root 4096 Jul 21 15:21 Terraform  
-rwxrwxrwx 1 root root   16 Jul 21 15:28 will_be_deleted.txt  
-rwxrwxrwx 1 root root   14 Jul 21 15:28 will_be_moved.txt  
devops-netology# git add will_be_deleted.txt  
devops-netology# git add will_be_moved.txt  
devops-netology# git commit -m "Prepare to delete and move"  
[main 23065dd] Prepare to delete and move  
 2 files changed, 2 insertions(+)  
 create mode 100644 will_be_deleted.txt  
 create mode 100644 will_be_moved.txt  

2.	В случае необходимости обратитесь к официальной документации: https://git-scm.com/book/ru/v2/Основы-Git-Запись-изменений-в-репозиторий , здесь подробно описано как выполнить последующие шаги.
3.	Удалите файл will_be_deleted.txt с диска и из репозитория.
devops-netology# git status  
On branch main  
Your branch is ahead of 'origin/main' by 3 commits.  
  (use "git push" to publish your local commits)  
  
Changes not staged for commit:  
  (use "git add/rm <file>..." to update what will be committed)  
  (use "git restore <file>..." to discard changes in working directory)  
        deleted:    will_be_deleted.txt  
  
no changes added to commit (use "git add" and/or "git commit -a")  
devops-netology# git rm will_be_deleted.txt  
rm 'will_be_deleted.txt'  
devops-netology# git status  
On branch main  
Your branch is ahead of 'origin/main' by 3 commits.  
  (use "git push" to publish your local commits)  
  
Changes to be committed:  
  (use "git restore --staged <file>..." to unstage)  
        deleted:    will_be_deleted.txt  
    
4.	Переименуйте (переместите) файл will_be_moved.txt на диске и в репозитории, чтобы он стал называться has_been_moved.txt.
devops-netology# git status  
On branch main  
Your branch is ahead of 'origin/main' by 3 commits.  
  (use "git push" to publish your local commits)  
  
Changes to be committed:  
  (use "git restore --staged <file>..." to unstage)  
        deleted:    will_be_deleted.txt  
  
Changes not staged for commit:  
  (use "git add/rm <file>..." to update what will be committed)  
  (use "git restore <file>..." to discard changes in working directory)  
        deleted:    will_be_moved.txt  
  
Untracked files:  
  (use "git add <file>..." to include in what will be committed)  
        has_been_moved.txt  
  
devops-netology# git status  
On branch main   
Your branch is ahead of 'origin/main' by 3 commits.  
  (use "git push" to publish your local commits)  
  
Changes to be committed:  
  (use "git restore --staged <file>..." to unstage)  
        renamed:    will_be_moved.txt -> has_been_moved.txt  
        deleted:    will_be_deleted.txt  
  
5.	Закоммитте результат работы с комментарием Moved and deleted.
devops-netology# git commit -m "Moved and deleted"  
[main e00be61] Moved and deleted  
 2 files changed, 1 deletion(-)  
 rename will_be_moved.txt => has_been_moved.txt (100%)  
 delete mode 100644 will_be_deleted.txt  

 Проверка изменений.
  
1.	В результате предыдущих шагов в репозитории должно быть как минимум пять коммитов (если вы еще сделали какие-нибудь промежуточные – нет проблем):
2.	Initial Commit – созданный гитхабом при инициализации репозитория.
3.	First commit – созданный после изменения файла README.md.
4.	Added gitignore – после добавления .gitignore.
5.	Prepare to delete and move – после добавления двух временных файлов.
6.	Moved and deleted – после удаления и перемещения временных файлов.
7.	Проверьте это используя комманду git log (подробно о формате вывода этой команды мы поговорим на следующем занятии, но посмотреть что она отображает можно уже сейчас).
  
commit e00be61cba621aad6c37ea857319b2d7edaad544 (HEAD -> main)  
Author: VitalyPetrakov <vitalik.petrakov@gmail.com>  
Date:   Wed Jul 21 16:37:44 2021 +0300  
  
    Moved and deleted  
  
commit 23065dd4b20800cf313cfaa4ed8952d17d08c906  
Author: VitalyPetrakov <vitalik.petrakov@gmail.com>  
Date:   Wed Jul 21 15:29:42 2021 +0300  
  
    Prepare to delete and move  
  
commit 8b7f83f4c36a7320e772f11f8163e69ef2a727ff  
Author: VitalyPetrakov <vitalik.petrakov@gmail.com>  
Date:   Wed Jul 21 15:24:06 2021 +0300  
  
    Added gitignore  
  
commit 64bf7cdc4d46f626326845ceb4c4d06dfc61f54e  
Author: VitalyPetrakov <vitalik.petrakov@gmail.com>  
Date:   Wed Jul 21 15:17:18 2021 +0300  
  
    First commit  
  
commit 1d2b61c341b0d54cc03450fd76b49bd84f7e1af5 (origin/main, origin/HEAD)  
Author: VitalikPetrakov <52289842+VitalikPetrakov@users.noreply.github.com>  
Date:   Wed Jul 21 14:49:05 2021 +0300  
  
    Initial commit  


