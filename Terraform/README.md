е задание к занятию «2.4. Инструменты Git»

Для выполнения заданий в этом разделе давайте склонируем репозиторий с исходным кодом 
терраформа https://github.com/hashicorp/terraform 

root@notebook:/mnt/c/devops/devops-netology# cd Terraform/  
root@notebook:/mnt/c/devops/devops-netology/Terraform# git clone https://github.com/hashicorp/terraform  
Cloning into 'terraform'...  
remote: Enumerating objects: 245165, done.  
remote: Counting objects: 100% (1038/1038), done.  
remote: Compressing objects: 100% (462/462), done.  
remote: Total 245165 (delta 619), reused 912 (delta 561), pack-reused 244127  
Receiving objects: 100% (245165/245165), 182.99 MiB | 862.00 KiB/s, done.  
Resolving deltas: 100% (151055/151055), done.  
Updating files: 100% (2805/2805), done.  

В виде результата напишите текстом ответы на вопросы и каким образом эти ответы были получены.   

1.	Найдите полный хеш и комментарий коммита, хеш которого начинается на `aefea`.  
root@notebook:/mnt/c/devops/devops-netology/Terraform/terraform# git show aefea  
commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545  
Author: Alisdair McDiarmid alisdair@users.noreply.github.com  
Date:   Thu Jun 18 10:29:58 2020 -0400  
  
    Update CHANGELOG.md  
  
diff --git a/CHANGELOG.md b/CHANGELOG.md  
index 86d70e3e0..588d807b1 100644  
--- a/CHANGELOG.md  
+++ b/CHANGELOG.md  
@@ -27,6 +27,7 @@ BUG FIXES:  
 * backend/s3: Prefer AWS shared configuration over EC2 metadata credentials by default ([#25134](https://github.com/hashicorp/terraform/issues/25134))  
 * backend/s3: Prefer ECS credentials over EC2 metadata credentials by default ([#25134](https://github.com/hashicorp/terraform/issues/25134))  
 * backend/s3: Remove hardcoded AWS Provider messaging   ([#25134](https://github.com/hashicorp/terraform/issues/25134))  
+* command: Fix bug with global `-v`/`-version`/`--version` flags introduced in 0.13.0beta2 [GH-25277]  
 * command/0.13upgrade: Fix `0.13upgrade` usage help text to include options ([#25127](https://github.com/hashicorp/terraform/issues/25127))  
 * command/0.13upgrade: Do not add source for builtin provider ([#25215](https://github.com/hashicorp/terraform/issues/25215))  
 * command/apply: Fix bug which caused Terraform to silently exit on Windows when using absolute plan path ([#25233](https://github.com/hashicorp/terraform/issues/25233))  
2. Какому тегу соответствует коммит `85024d3`?  
root@notebook:/mnt/c/devops/devops-netology/Terraform/terraform# git show 85024d3  
commit 85024d3100126de36331c6982bfaac02cdab9e76 (tag: v0.12.23)  
3.	Сколько родителей у коммита `b8d720`? Напишите их хеши.  
56cd7859e05c36c06b56d013b55a252d0bb7e158   
        9ea88f22fc6269854151c571162c5bcf958bee2b  
4.	Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами  v0.12.23 и v0.12.24.
root@notebook:/mnt/c/devops/devops-netology/Terraform/terraform# git log --oneline --graph v0.12.23 v0.12.24  
* 33ff1c03b (tag: v0.12.24) v0.12.24  
* b14b74c49 [Website] vmc provider links  
* 3f235065b Update CHANGELOG.md  
* 6ae64e247 registry: Fix panic when server is unreachable  
* 5c619ca1b website: Remove links to the getting started guide's old location  
* 06275647e Update CHANGELOG.md  
* d5f9411f5 command: Fix bug when using terraform login on Windows  
* 4b6d06cc5 Update CHANGELOG.md  
* dd01a3507 Update CHANGELOG.md  
* 225466bc3 Cleanup after v0.12.23 release  
* 85024d310 (tag: v0.12.23) v0.12.23  
5. Найдите коммит в котором была создана функция `func providerSource`, ее определение в коде выглядит 
так `func providerSource(...)` (вместо троеточего перечислены аргументы).
root@notebook:/mnt/c/devops/devops-netology/Terraform/terraform# git log --pretty=oneline -S "func providerSource"  
5af1e6234ab6da412fb8637393c5a17a1b293663 main: Honor explicit provider_installation CLI config when present  
8c928e83589d90a031f811fae52a81be7153e82f main: Consult local directories as potential mirrors of providers  
После просмотра этих коммитов по хешам, судя по датам первый коммит, кто ввел эту функцию  
root@notebook:/mnt/c/devops/devops-netology/Terraform/terraform# git show 8c928e835  
commit 8c928e83589d90a031f811fae52a81be7153e82f  
Author: Martin Atkins mart@degeneration.co.uk  
Date:   Thu Apr 2 18:04:39 2020 -0700  
6.	Найдите все коммиты в которых была изменена функция `globalPluginDirs`.  
root@notebook:/mnt/c/devops/devops-netology/Terraform/terraform# git log -S "globalPluginDirs" --source --all –oneline  
35a058fb3       refs/tags/v0.10.8 main: configure credentials from the CLI config file  
c0b176109       refs/tags/v0.10.0-beta2 prevent log output during init  
8364383c3       refs/tags/v0.10.0-beta2 Push plugin discovery down into command package  
warning: inexact rename detection was skipped due to too many files.  
warning: you may want to set your diff.renameLimit variable to at least 467 and retry the   command.  


!!!!Видимо я чтото делаю не так, вряд ли надо вывести все 467 коммитов  !!!!   
!!!!Я пробовал найти все файлы, там было не на много лучше  !!!!   
  
7. Кто автор функции `synchronizedWriters`?   
root@notebook:/mnt/c/devops/devops-netology/Terraform/terraform# git log --all -p --reverse --source -S "synchronizedWriters"  
commit 5ac311e2a91e381e2f52234668b49ba670aa0fe5 refs/tags/v0.9.5  
Author: Martin Atkins mart@degeneration.co.uk  
Date:   Wed May 3 16:25:41 2017 -0700  
