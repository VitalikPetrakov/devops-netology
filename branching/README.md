 Домашнее задание к занятию «2.3. Ветвления в Git»
Давайте потренеруемся делать merge и rebase, чтобы понять разницу и научиться решать конфликты.
Задание №1 – Ветвление, merge и rebase.
1.	Предположим, что есть задача написать скрипт, выводящий на экран параметры его запуска. Давайте посмотрим, как будет отличаться работа над этим скриптом с использованием ветвления, мержа и ребейза. Создайте в своем репозитории каталог branching и в нем два файла merge.sh и rebase.sh с содержимым:                          
    #!/bin/bash     
    # display command line options

count=1
for param in "$*"; do
    echo "\$* Parameter #$count = $param"
    count=$(( $count + 1 ))
done
Этот скрипт отображает на экране все параметры одной строкой, а не разделяет их.
2.	Создадим коммит с описанием prepare for merge and rebase и отправим его в ветку main.
devops-netology# git commit -m "prepare for merge and rebase"  
devops-netology# git push  
Username for 'https://github.com': vitalik.petrakov@gmail.com  
Password for 'https://vitalik.petrakov@gmail.com@github.com':  
Enumerating objects: 9, done.  
Counting objects: 100% (9/9), done.  
Delta compression using up to 6 threads  
Compressing objects: 100% (7/7), done.  
Writing objects: 100% (8/8), 836 bytes | 119.00 KiB/s, done.  
Total 8 (delta 2), reused 0 (delta 0)  
remote: Resolving deltas: 100% (2/2), completed with 1 local object.  
To https://github.com/VitalikPetrakov/devops-netology.git  
   3736c56..793a777  main -> main  
Подготовка файла merge.sh.
1.	Создайте ветку git-merge.
devops-netology# git checkout -b git-merge  
2.	Замените в ней содержимое файла merge.sh на       
    #!/bin/bash              
    # display command line options       

count=1
for param in "$@"; do
    echo "\$@ Parameter #$count = $param"
    count=$(( $count + 1 ))
done
devops-netology# nano branching/merge.sh  
devops-netology# git add branching/merge.sh  
devops-netology# git status  
On branch git-merge  
Changes to be committed:  
  (use "git restore --staged <file>..." to unstage)  
        modified:   branching/merge.sh  
3.	Создайте коммит merge: @ instead * отправьте изменения в репозиторий.
devops-netology# git push --set-upstream origin git-merge  
Username for 'https://github.com': vitalik.petrakov@gmail.com  
Password for 'https://vitalik.petrakov@gmail.com@github.com':  
Enumerating objects: 7, done.  
Counting objects: 100% (7/7), done.  
Delta compression using up to 6 threads  
Compressing objects: 100% (4/4), done.  
Writing objects: 100% (4/4), 430 bytes | 107.00 KiB/s, done.  
Total 4 (delta 2), reused 0 (delta 0)  
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.  
remote:  
remote: Create a pull request for 'git-merge' on GitHub by visiting:  
remote:      https://github.com/VitalikPetrakov/devops-netology/pull/new/git-merge  
remote:  
To https://github.com/VitalikPetrakov/devops-netology.git  
 * [new branch]      git-merge -> git-merge  
Branch 'git-merge' set up to track remote branch 'git-merge' from 'origin'.  
4.	И разработчик подумал и решил внести еще одно изменение в merge.sh  
"#!/bin/bash           
"# display command line options

count=1  
while [[ -n "$1" ]]; do  
    echo "Parameter #$count = $1"  
    count=$(( $count + 1 ))  
    shift  
done  
Теперь скрипт будет отображать каждый переданный ему параметр отдельно.
5.	Создайте коммит merge: use shift и отправьте изменения в репозиторий.
devops-netology# nano branching/merge.sh  
devops-netology# git add *  
devops-netology# git commit -m "merge: use shift"  
[git-merge 4331aee] merge: use shift  
 1 file changed, 3 insertions(+), 2 deletions(-)  
devops-netology# git push  
Username for 'https://github.com': vitalik.petrakov@gmail.com  
Password for 'https://vitalik.petrakov@gmail.com@github.com':  
Enumerating objects: 7, done.  
Counting objects: 100% (7/7), done.  
Delta compression using up to 6 threads  
Compressing objects: 100% (4/4), done.  
Writing objects: 100% (4/4), 500 bytes | 166.00 KiB/s, done.  
Total 4 (delta 1), reused 0 (delta 0)  
remote: Resolving deltas: 100% (1/1), completed with 1 local object.  
To https://github.com/VitalikPetrakov/devops-netology.git  
   bf8560f..4331aee  git-merge -> git-merge  

 
Изменим main.
1.	Вернитесь в ветку main.
devops-netology# git checkout origin/main  
Note: switching to 'origin/main'.  
You are in 'detached HEAD' state. You can look around, make experimental  
changes and commit them, and you can discard any commits you make in this  
state without impacting any branches by switching back to a branch.  
If you want to create a new branch to retain commits you create, you may  
do so (now or later) by using -c with the switch command. Example:  
  git switch -c <new-branch-name>  
Or undo this operation with:  
  git switch -  
Turn off this advice by setting config variable advice.detachedHead to false  
HEAD is now at 793a777 prepare for merge and rebase
2.	Предположим, что кто-то, пока мы работали над веткой git-merge, изменил main. Для этого изменим содержимое файла rebase.sh на следующее    
"#!/bin/bash            
"# display command line options    
  
count=1  
for param in "$@"; do  
    echo "\$@ Parameter #$count = $param"  
    count=$(( $count + 1 ))  
done  

echo "====="  
В этом случае скрипт тоже будет отображать каждый параметр в новой строке.
3.	Отправляем измененную ветку main в репозиторий.
devops-netology# git push origin HEAD:main  
Username for 'https://github.com': vitalik.petrakov@gmail.com  
Password for 'https://vitalik.petrakov@gmail.com@github.com':  
Enumerating objects: 7, done.  
Counting objects: 100% (7/7), done.  
Delta compression using up to 6 threads  
Compressing objects: 100% (4/4), done.  
Writing objects: 100% (4/4), 388 bytes | 129.00 KiB/s, done.  
Total 4 (delta 3), reused 0 (delta 0)  
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.  
To https://github.com/VitalikPetrakov/devops-netology.git  
   793a777..23668dd  HEAD -> main  
Подготовка файла rebase.sh.
1.	Предположим, что теперь другой участник нашей команды не сделал git pull, либо просто хотел ответвиться не от последнего коммита в main, а от коммита когда мы только создали два файла merge.sh и rebase.sh на первом шаге.
Для этого при помощи команды git log найдем хэш коммита prepare for merge and rebase и выполним git checkout на него примерно так: git checkout 8baf217e80ef17ff577883fda90f6487f67bbcea (хэш будет другой).
devops-netology# git checkout   793a777cace8679b0a969152e20347c0ebc88a20  
Previous HEAD position was 23668dd show new param in a new line  
HEAD is now at 793a777 prepare for merge and rebase  
2.	Создадим ветку git-rebase основываясь на текущем коммите.
3.	И изменим содержимое файла rebase.sh на следующее, тоже починив скрипт, но немного в другом стиле     

"#!/bin/bash  
"# display command line options  

count=1  
for param in "$@"; do  
    echo "Parameter: $param"  
    count=$(( $count + 1 ))  
done  

echo "====="  
4.	Отправим эти изменения в ветку git-rebase, с комментарием git-rebase 1.
5.	И сделаем еще один коммит git-rebase 2 с пушем заменив echo "Parameter: $param" на echo "Next parameter: $param".
devops-netology# git checkout -b git-rebase  
Switched to a new branch 'git-rebase'  
devops-netology# nano branching/rebase.sh  
devops-netology# git add branching/rebase.sh  
devops-netology# git commit -m "git-rebase 1"  
[git-rebase 1bb39e1] git-rebase 1  
 1 file changed, 4 insertions(+), 2 deletions(-)  
devops-netology# nano branching/rebase.sh  
devops-netology# git add branching/rebase.sh  
devops-netology# git commit -m "git-rebase 2"  
devops-netology# git push origin git-rebase    
Username for 'https://github.com': vitalik.petrakov@gmail.com  
Password for 'https://vitalik.petrakov@gmail.com@github.com':    
Enumerating objects: 11, done.                
Counting objects: 100% (11/11), done.        
Delta compression using up to 6 threads           
Compressing objects: 100% (8/8), done.             
Writing objects: 100% (8/8), 740 bytes | 105.00 KiB/s, done.   
Total 8 (delta 5), reused 0 (delta 0)                      
remote: Resolving deltas: 100% (5/5), completed with 2 local objects.  
To https://github.com/VitalikPetrakov/devops-netology     
   793a777..6c1c8d9  git-rebase -> git-rebase         
 To https://github.com/VitalikPetrakov/devops-netology.git    
 * [new branch]      git-rebase -> git-rebase    
Промежуточный итог.
Мы сэмулировали типичную ситуации в разработке кода, когда команда разработчиков работала над одним и тем же участком кода, причем кто-то из разработчиков предпочитаем делать merge, а кто-то rebase. Конфилкты с merge обычно решаются достаточно просто, а с rebase бывают сложности, поэтому давайте смержим все наработки в main и разрешим конфликты.
Если все было сделано правильно, то на странице network в гитхабе, находящейся по адресу https://github.com/ВАШ_ЛОГИН/ВАШ_РЕПОЗИТОРИЙ/network будет примерно такая схема:  

 
Merge
Сливаем ветку git-merge в main и отправляем изменения в репозиторий, должно получиться без конфликтов:
$ git merge git-merge
Merge made by the 'recursive' strategy.    
 branching/merge.sh | 5 +++--               
 1 file changed, 3 insertions(+), 2 deletions(-)    
$ git push                                        
@#!/bin/bash       
Enumerating objects: 1, done.  
Counting objects: 100% (1/1), done.  
Writing objects: 100% (1/1), 223 bytes | 223.00 KiB/s, done.  
Total 1 (delta 0), reused 0 (delta 0), pack-reused 0  
В результате получаем такую схему:  
devops-netology# git checkout origin/main  
devops-netology# git merge git-merge  
Merge made by the 'recursive' strategy.  
 branching/merge.sh | 5 +++--  
 1 file changed, 3 insertions(+), 2 deletions(-)  
devops-netology# git push origin HEAD:main  
Username for 'https://github.com': vitalik.petrakov@gmail.com  
Password for 'https://vitalik.petrakov@gmail.com@github.com':  
Enumerating objects: 7, done.  
Counting objects: 100% (7/7), done.  
Delta compression using up to 6 threads  
Compressing objects: 100% (3/3), done.  
Writing objects: 100% (3/3), 361 bytes | 120.00 KiB/s, done.  
Total 3 (delta 2), reused 0 (delta 0)  
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.  
To https://github.com/VitalikPetrakov/devops-netology  
   7c4f781..ceb2c97  HEAD -> main  
Rebase
1.	А перед мержем ветки git-rebase выполним ее rebase на main. Да, мы специально создали ситуацию с конфликтами, чтобы потренироваться их решать.
2.	Переключаемся на ветку git-rebase и выполняем git rebase -i main. В открывшемся диалоге должно быть два выполненных нами коммита, давайте заодно объединим их в один, указав слева от нижнего fixup. В результате получаем что-то подобное:
$ git rebase -i main
Auto-merging branching/rebase.sh
CONFLICT (content): Merge conflict in branching/rebase.sh
error: could not apply dc4688f... git 2.3 rebase @ instead *
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply dc4688f... git 2.3 rebase @ instead *
Если посмотреть содержимое файла rebase.sh, то увидим метки, оставленные гитом для решения конфликта:


"cat rebase.sh                         "    
"#!/bin/bash                           "    
"# display command line options        "    
"count=1                               "      
"for param in "$@"; do                 "      
"<<<<<<< HEAD                          "       
"    echo "\$@ Parameter #$count = $param""     
"=======                               "     
"    echo "Parameter: $param"          "      
">>>>>>> dc4688f... git 2.3 rebase @ instead *"      
"    count=$(( $count + 1 ))           "         
"done                                  "         
"Удалим метки, отдав предпочтение варианту   "
"echo "\$@ Parameter #$count = $param" "        


devops-netology# git branch -D tmpmain  
Deleted branch tmpmain (was ceb2c97).  
devops-netology# git rebase -i origin/main  
Auto-merging branching/rebase.sh  
CONFLICT (content): Merge conflict in branching/rebase.sh  
error: could not apply 557482b... git-rebase 1  
Resolve all conflicts manually, mark them as resolved with  
"git add/rm <conflicted_files>", then run "git rebase --continue".  
You can instead skip this commit: run "git rebase --skip".  
To abort and get back to the state before "git rebase", run "git rebase --abort".  
Could not apply 557482b... git-rebase 1  
devops-netology# git status  
interactive rebase in progress; onto ceb2c97  
Last command done (1 command done):  
   p 557482b git-rebase 1  
Next command to do (1 remaining command):  
   f 6c1c8d9 git-rebase 2  
  (use "git rebase --edit-todo" to view and edit)  
You are currently rebasing branch 'git-rebase' on 'ceb2c97'.  
  (fix conflicts and then run "git rebase --continue")  
  (use "git rebase --skip" to skip this patch)  
  (use "git rebase --abort" to check out the original branch)  

Unmerged paths:  
  (use "git restore --staged <file>..." to unstage)  
  (use "git add <file>..." to mark resolution)  
        both modified:   branching/rebase.sh  
  
no changes added to commit (use "git add" and/or "git commit -a")  
devops-netology# nano branching/rebase.sh  
devops-netology# nano branching/rebase.sh  
сообщим гиту, что конфликт решен git add rebase.sh и продолжим ребейз git rebase --continue.
И опять в получим конфликт в файле rebase.sh при попытке применения нашего второго коммита. Давайте разрешим конфликт, оставив строчку echo "Next parameter: $param".
Далее опять сообщаем гиту о том, что конфликт разрешен git add rebase.sh и продолжим ребейз git rebase --continue. В результате будет открыт текстовый редактор предлагающий написать комментарий к новому объединенному коммиту:
                         
This is a combination of 2 commits.    
This is the 1st commit message:      
Merge branch 'git-merge'             
The commit message #2 will be skipped:   
git 2.3 rebase @ instead * (2)        

Все строчки начинающиеся на # будут проигнорированны.
После сохранения изменения, гит сообщит
Successfully rebased and updated refs/heads/git-rebase

devops-netology# git rebase –continue  
branching/rebase.sh: needs merge  
You must edit all merge conflicts and then  
mark them as resolved using git add  
devops-netology# git status  
interactive rebase in progress; onto ceb2c97  
Last command done (1 command done):  
   p 557482b git-rebase 1  
Next command to do (1 remaining command):  
   f 6c1c8d9 git-rebase 2  
  (use "git rebase --edit-todo" to view and edit)  
You are currently rebasing branch 'git-rebase' on 'ceb2c97'.  
  (fix conflicts and then run "git rebase --continue")  
  (use "git rebase --skip" to skip this patch)  
  (use "git rebase --abort" to check out the original branch)  

Unmerged paths:  
  (use "git restore --staged <file>..." to unstage)  
  (use "git add <file>..." to mark resolution)  
        both modified:   branching/rebase.sh  

no changes added to commit (use "git add" and/or "git commit -a")  
devops-netology# git add branching/rebase.sh  
devops-netology# git rebase –continue  
Auto-merging branching/rebase.sh  
CONFLICT (content): Merge conflict in branching/rebase.sh  
error: could not apply 6c1c8d9... git-rebase 2  
Resolve all conflicts manually, mark them as resolved with  
"git add/rm <conflicted_files>", then run "git rebase --continue".  
You can instead skip this commit: run "git rebase --skip".  
To abort and get back to the state before "git rebase", run "git rebase --abort".  
Could not apply 6c1c8d9... git-rebase 2   
devops-netology# git status  
interactive rebase in progress; onto ceb2c97  
Last commands done (2 commands done):  
   p 557482b git-rebase 1  
   f 6c1c8d9 git-rebase 2  
No commands remaining.  
You are currently rebasing branch 'git-rebase' on 'ceb2c97'.  
  (fix conflicts and then run "git rebase --continue")  
  (use "git rebase --skip" to skip this patch)  
  (use "git rebase --abort" to check out the original branch)  

Unmerged paths:  
  (use "git restore --staged <file>..." to unstage)  
  (use "git add <file>..." to mark resolution)  
        both modified:   branching/rebase.sh  

no changes added to commit (use "git add" and/or "git commit -a")  
devops-netology# nano branching/rebase.sh  
devops-netology# git add branching/rebase.sh  
devops-netology# git rebase –continue  
Successfully rebased and updated refs/heads/git-rebase.  
И попробуем выполнить git push, либо git push -u origin git-rebase чтобы точно указать что и куда мы хотим запушить. Эта команда завершится с ошибкой:
git push
To github.com:andrey-borue/devops-netology.git
 ! [rejected]        git-rebase -> git-rebase (non-fast-forward)
error: failed to push some refs to 'git@github.com:andrey-borue/devops-netology.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
Это произошло, потому что мы пытаемся перезаписать историю. Чтобы гит позволил нам это сделать, давайте добавим флаг force:
git push -u origin git-rebase -f
Enumerating objects: 10, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 12 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 443 bytes | 443.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:andrey-borue/devops-netology.git
 + 1829df1...e3b942b git-rebase -> git-rebase (forced update)
Branch 'git-rebase' set up to track remote branch 'git-rebase' from 'origin'.
Теперь можно смержить ветку git-rebase в main без конфликтов и без дополнительного мерж-комита простой перемоткой.
$ git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

$ git merge git-rebase
Updating 6158b76..45893d1
Fast-forward
 branching/rebase.sh | 3 +--
 1 file changed, 1 insertion(+), 2 deletions(-)
devops-netology# git push -u origin git-rebase –f  
Username for 'https://github.com': vitalik.petrakov@gmail.com  
Password for 'https://vitalik.petrakov@gmail.com@github.com':  
Total 0 (delta 0), reused 0 (delta 0) 
To https://github.com/VitalikPetrakov/devops-netology  
 + 6c1c8d9...ceb2c97 git-rebase -> git-rebase (forced update)  
Branch 'git-rebase' set up to track remote branch 'git-rebase' from 'origin'.  
devops-netology# git checkout main  
Switched to branch 'main'  
Your branch is behind 'origin/main' by 4 commits, and can be fast-forwarded.  
  (use "git pull" to update your local branch)  
devops-netology# git merge main  
Already up to date.  
devops-netology# git pull  
Updating 793a777..ceb2c97  
Fast-forward  
 branching/merge.sh  | 5 +++--  
 branching/rebase.sh | 6 ++++--  
 2 files changed, 7 insertions(+), 4 deletions(-)  
devops-netology# git status  
On branch main  
Your branch is up to date with 'origin/main'.  

nothing to commit, working tree clean  
devops-netology# git merge git-rebase  
Already up to date.  
Цель задания - попробовать на практике то, как выглядит решение конфликтов. Обычно при нормальном ходе разработки выполнять rebase достаточно просто, что позволяет объединить множество промежуточных коммитов при решении задачи, чтобы не засорять историю, поэтому многие команды и разработчики предпочитают такой способ.
Есть еще такой тренажер https://learngitbranching.js.org/, где можно потренироваться в работе с деревом коммитов и ветвлений.


