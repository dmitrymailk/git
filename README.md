## git

- Created: 2021-06-20 14:02
- Tags: #git
- Link: https://git-scm.com/book/en/v2
- Момент появления:
- Summary:

## Local Version Control Systems

- tags #LVCS

---

Одна из первых систем контроля версий это просто копирование файлов в арзив и называение их согласно дате и времени, это не совсем корректно. Помочь этому должны специальные системы Local Version Control. Наиболее используемая для этого программа называется [RCS](https://www.gnu.org/software/rcs/). Но она годится только для одного человека(как я понял)

---

## Centralized Version Control Systems

- tags #CVCS

---

Со временем людям потребовалось координировать свои усилия, поэтому была создана Centralized Version Control System. В ней есть главный сервер который хранит все изменения, а все остальные клиенты синхронизируются с ним. Долгое время это был стандарт. Но только у такого подхода существует множество проблем:

- диск сервера сломался, все данные утеряны
- сервер не доступен, никто не может сохранить свою работу
  Данные проблемы были и в LVCS, только там все хранилось на одном компьютере локально.

---

## Distributed Version Control Systems

- tags #DVCS

---

Distributed Version Control Systems (Git, Mercurial, Bazaar or Darcs) были следующим шагом в этом направлении. Клиенты имели не только последнюю версию проекта, но и всю его историю изменений. Это позволило восстановить весь проект, в случае полного краха главного сервера, так как каждый клиент, который копировал проект имел полную историю его изменений и состояний.

---

## A Short History of Git

- tags

---

Сначала работали с программой которая называлась BitKeeper. Потом она перестала быть бесплатной и сообщество Linux решило сделать свою систему, которая называется GIT.

---

## Snapshots, Not Differences

- tags #snapshots

---

Большинство систем подобных Git используют так называемую delta-based version control (CVS, Subversion, Perforce, Bazaar), это значит что они хранят изменения исходных файлов.
Git работает с данными иначе. Каждая новая версия для него это слепок всей файловой системы. После каждого коммита, он сохраняет полное состояние всего проекта. Если какие-то файлы не меняются в процессе разработки, то он оставляет на них только ссылки. Git представляет данные как последовательность состояний проекта.

---

## Nearly Every Operation Is Local

- tags

---

В отличии от CVCS, так как git хранит большую часть инфрмации о проекте локально, то и все операции по изменению можно выполнять локально. А потом когда все будет готово, нужно будет лишь загрузить все изменения на сервер.

---

## Git Has Integrity

- tags

---

Каждый файл в git имеет механизм вычисления контрольной суммы, это значит что каждому изменению файла будет присвоен свой ID.

---

## The Three States

- tags #git

---

git имеет 3 основных состояния:

- modified - файл изменен, но его изменения не сохранены в базе данных
- staged - значит что файл помечет для переноса в коммит
- committed - значит что файл сохранен в локально базе данных
  Соответсвенно существуют 3 секции
- working tree - непосредственно сами файлы с которыми работает программист
- staging area - мета информация для следующего коммита которая храниться в git directory
- git directory - дирректория где хранятся все важные файлы изменений проекта. Она копируется с сервера, когда исполняется команда **clone**

Обычный сценарий работы с git:

1. изменяешь файлы в working tree
2. добавляешь те файлы которые хочешь сохранить в staging area
3. делаешь коммит и сохраняешь состояние своего проекта

---

## First-Time Git Setup

- tags #git #git_setup

---

Инструмент который позволит настоить сам гит называется

- git config

### Узнать все настройки можно при помощи команды

```console
git config --list --show-origin
```

### Установить name и email

```console
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

Дирректива --global обозначает что эти параметры будут использоваться в каждом проекте на компьютере.

### Установить редактор по умолчанию

```console
git config --global core.editor emacs
```

### Установить ветку по умолчанию с названием main

```console
git config --global init.defaultBranch main
```

### Узнать все значения параметров к config

```console
git config --list
```

Иногда можно увидеть ключи более одного раза, потому что Git читает один и тот же ключ из разных файлов.

---

## Getting Help

- tags #git #git_help

---

### Открыть веб страницу с подробной документацией инструмента

```console
git help config
```

### Открыть список доступных команд для инструмента в консоли

```console
git add -h
```

---

## Getting a Git Repository

- tags #git #git_repository

---

### Инициализация репозитория

```console
git init
```

### Добавление всех файлов расширения С

```console
git add *.c
```

### Коммит всех изменений с комментарием

```console
git commit -m 'Initial project version'
```

### Клонировать репозитрий

```console
git clone https://github.com/libgit2/libgit2
```

### Клонировать репозитрий с кастомным наименованием папки где будет хранится проект

mylibgit - имя папки

```console
git clone https://github.com/libgit2/libgit2 mylibgit
```

---

## Recording Changes to the Repository

- tags #git #git_repository

---

Файл может находиться в двух состояниях

- tracked - с этими файлами git работает
- untracked - эти он игнорирует и они не идут в staged area

### Узнать состояние файлов в репозитории

```console
git status
```

### Добавить для отслеживания новый файл

```console
git add README
```

### Игнорирование файлов

Специальный файл .gitignore служит для того чтобы git не отслеживал специальные файлы. Виды неотслеживаемых файлов можно записывать ввиде паттернов

```plaintext
# ignore all .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in any directory named build
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```

### Автодобавление отслеживаемых файлов и их коммит

```console
git commit -a -m 'new commit'
```

### Удалить удаленный файл из отслеживания

```console
git rm PROJECTS.md
```

### Удалить файл из отслеживания, если к примеру мы забыли указать его в .gitignore

```console
git rm --cached file_name
```

### Перемещение файлов

В отличие от других VCS, git не отслеживает явно перемещения файлов из одной дирректории в другую. Если имя какого-то файла изменится, то об этом изменении нигде не будет написано. Для того чтобы переименовать файл в git нужно написать следующую команду

```console
git mv file_from file_to
```

---

## Viewing the Commit History

- tags #git #git_commit

---

### Посмотреть историю коммитов

```console
git log
```

### Посмотреть историю последених 2 коммитов с показанной разницей

```console
git log -p -2
```

### Показать статистику добавлений и удалений для коммитов

```console
git log --stat
```

### Показать комментарии к коммитам и их хеши

```console
git log --pretty=oneline
```

### Показать короткие хеши, автора, время коммита и комментарий к нему

Подробнее насчет управляющих последовательностей [тут](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)

```console
git log --pretty=format:"%h - %an, %ar : %s"
```

---

## Undoing Things

- tags #git #git_undo

---

### Отредактировать сообщение в недавнем коммите

```console
git commit --amend
```

### Удалить только что добавленный файл из staged

```console
git restore --staged file_name
```

### Безвозвратно вернуть файл к прошлому коммиту через checkout

```console
git checkout -- file_name
```

### Безвозвратно вернуть файл к прошлому коммиту через restore

```console
git restore file_name
```

---

## Working with Remotes

- tags #git #git_remote

---

### Узнать адреса с которых происходит скачивание и пушинг

```console
git remote -v
```

### Добавить origin удаленного репозитория в уже имеющийся локальный

Кстати origin добавляется автоматически неявно когда выполняется команда git clone <br/>

- shortname - имя по которому будет вызываться данный url, задается кастомно

```console
git remote add shortname url
```

Пример

```console
git remote add pb https://github.com/paulboone/ticgit
```

### Получить данные с удаленного репозитория

Когда клонируется репозиторий, то ему автоматически присваивается имя origin.<br/>
При выполнении этой команды файлы не сливаются автоматически.

```console
git fetch <remote>
```

### Получить данные с удаленного репозитория и слить их с текущими

```console
git pull
```

### Отправить данные на сервер в нужную ветку

git push remote branch

- remote - origin
- master - ветка

```console
git push origin master
```

### Узнать ветки и url удаленного репозитория

```console
git remote show origin
```

### Переименовать remote’s shortname

```console
git remote rename old_name new_name
```

### Удалить ненужный remote

```console
git remote remove remote_short_name
```

---

## Tagging

- tags

---

Создание тегов может быть полезно для версионирования продукта, к примеру можно написать v1.0, v2.0 и тд. Иными словами теги это способ как-то обозначить особые коммиты.Git поддерживает 2 вида тегов

- lightweight
- annotated
- ![](../../images/Pasted%20image%2020210622213938.png)
- ![](../../images/Pasted%20image%2020210622213955.png)

### Отобразить весь список тегов

```console
git tag
```

### Создать тег содержащий сообщение (annotated)

```console
git tag -a v1.4 -m "my version 1.4"
```

### Показать информацию о теге и коммите прикрепленном к нему

```console
git show v1.4
```

### Создать обычный тег без всего, содержащий только информацию о контрольной сумме (lightweight)

```console
git tag v1.4-lw
```

### Добавить тег к уже существующему коммиту

```console
git tag -a v1.2 9fceb02
```

### Добавление созданного тега на удаленный сервер

```console
git push origin v1.5
```

### Добавление всех созданных тегов на удаленный сервер

```console
git push origin --tags
```

### Удаление тегов локально

```console
git tag -d v0.3-lw
```

### Удаление тегов на сервере

- git push **_remote_** :refs/tags/**_tagname_**

```console
git push origin :refs/tags/v1.4-lw
```

### Если нужно исправить ошибку в старой версии тега, то нужно создать новую ветку со старым тегом

```console
git checkout -b version2 v2.0.0
```

---

## Git Aliases

- tags #git #git_aliases

---

Git позволяет делать некоторые горячие клавиши, чтобы переназначать оригинальные команды или комбинировать множество. Это можно и не делать, все зависит от вкуса.

---

## Branches in a Nutshell

- tags

---

Ветка это указатель на какой-то определенный коммит.

### Создать новую ветку (old)

Она создает указатель на текущий коммит на котором ты сейчас находишься. Гит знает на какой ветке ты сейчас находишься через специальный указатель HEAD.

```console
git branch branch_name
```

### Создать новую ветку и переключиться на нее (new)

```console
git switch -c branch_name
```

### Переключится на нужную ветку (old)

```console
git checkout branch_name
```

### Переключится на нужную ветку (new)

```console
git switch branch_name
```

### Показать на какой ветке находишься сейчас

HEAD указывает на какой ветке ты находишься сейчас.

```console
git log --oneline
```

---

## Basic Branching and Merging

- tags

---

### Смержить какую-то ветку с текущей в одну

```console
git merge branch_name
```

### Удалить ветку

```console
git branch -d branch_name
```

### Открыть инструмент для решения merge конфликта

```console
git mergetool
```

---

## Branch Management

- tags

---

### Показать список веток с их описанием

```console
git branch -v
```

### Показать список всех веток

```console
git branch --all
```

### Переименовать локальную ветку

```console
git branch --move bad-branch-name corrected-branch-name
```

### Переименовать ветку на сервере

```console
git push --set-upstream origin corrected-branch-name
```

### Удалить ветку на сервере

```console
git push origin --delete bad-branch-name
```

---

## Branching Workflows

- tags

---

Стандартная практика в разработке иметь несколько веток. Одна ветка к примеру самая главная и крутится в продакшн, вторая для разработки и постоянных улучшений, а 3 вообще для какой-то фичи.

---

## Remote Branches

- tags

---

### Добавить новую локальную ветку на сервер

```console
git push remote_name branch_name
```

### Связать локальную ветку связанную с удаленной, tracked branch

```console
git checkout --track remote_name/branch_name
```

### Связать существующую ветку с удаленной

```console
git branch -u remote_name/branch_name
```

---

## Rebasing

- tags #git #git_rebase

---

Существует несколько возможностей добавить изменения одной ветки в другую

- merge
- rebase
  Rebase нужен для работы над фича ветками. Он подходит только тогда, когда над веткой работает ТОЛЬКО один человек, иначе при пуше rebase ветки остальные разработчики получат конфликты. Если над веткой работает много человек лучше всего использовать merge. Merge делает некрасивым историю взаимодействия с веткой.

Для чего же нужны они для фича веток. К примеру решили пофиксить баг или добавить фичу. Почкуемся на другую ветку и делаем на ней свои дела, но дело в том что на той ветке, от которой отпочковались тоже ведуться изменения и нам нужно как-то обновлять фича ветку. Так вот merge будет объединять результаты и делать новый коммит, а rebase будет перемещать всю историю после той ветки от которой отпочковались и перезаписывать хеши что может повлечь конфликты для других разработчиков.

### Сделать rebase с какой-то веткой

```console
git rebase branch_name
```

**Не перемещайте коммиты, уже отправленные в публичный репозиторий
Если вы будете придерживаться этого правила, всё будет хорошо. Если не будете, люди возненавидят вас, а ваши друзья и семья будут вас презирать.**

---

## Distributed Workflows

- tags

---

### Centralized Workflow

Существует только одно место где содердится код и каждый участник синхронизируется с ним. Каждый разработчик является узлом который связан только с одним местом. Это значит что если 2 разработчика скачали репозиторий и сделали в нем какие-то изменения. После того как первый загрузил свои результаты, второму, который хочет тоже добавить свои наработки должен слить код с первым и только затем загрузить свой результат на сервер. На случай если 2 человека заходят загрузить код якобы одновременно, гит запретит второму загружать, пока первый не закончит.

### Integration-Manager Workflow

Существует и другой сценарий работы. Когда не каждый может добавлять изменения в проект, а только владелец репозитория, но читать этот репозиторий могут все. И если кто-то хочет развить проект он клонирует репозиторий, делает какие-то изменения и потом пишет владельцу мол смотри я сделал. Владелец клонирует локально к себе на комп измененный репозиторий, смотрит его, если его все устраивает, то он сливает его и загружает изменения на изначальный репозиторий.
На github это значит то ты сделал fork проекта.

### Dictator and Lieutenants Workflow

Данная стратегия была разработана для действительно большиз проектов, по типу ядра линукс. Главный Репозиторий(Dictator) бъется на какие-то зоны ответствености(Lieutenants).И когда что-то сделано, тогда ветки Lieutenants сливаются с Dictator. Обычные разработчики работают только над своими ветками и сливаются с Lieutenant ветками. Данная стратегия не очень популярна и подходит только для действительно больших проектов подобно линукс.

---

## Forked Public Project

- tags

---

### Клонировать чей-то репозиторий для улучшения и создания

```
git clone <url>
git checkout -b featureA
git remote add fork_name <url>
```

### Загрузить свой fork на сервер

```console
git push -u fork_name featureA
```

### Сделать pull request, когда работа завершена

```console
git request-pull origin/master myfork
```

---

## Maintaining a Project

- tags

---

Если вы являетесь основателем репозитория и принимаете от других какие-то изменения, то лучше всего прежде чем их принимать протестить на новой ветке и убедиться, что все хорошо.

### Принять патч который был сгенерирован при помощи git diff(лучше не делать при помощи этой штуки)

```console
git apply /tmp/patch-ruby-client.patch
```

### Проверить корректно ли применится патч

```console
git apply --check 0001-see-if-this-helps-the-gem.patch
```

### Принять патч, который был сгенерирован при помощи format-patch

```console
git am 0001-limit-log-function.patch
```

### Принять патч, с разрешением rebase & merge конфликтов

```console
git am --resolved
```

### Узнать что произойдет с этой веткой если ее merge с другой

```console
git diff master
```

### Показать что добавится при merge 2 веток, 1 ветка это та с которой произойдет слияние, 2 ветка это ветка содержащая изменения

```console
git diff old_branch...feature_brunch
```

```console
git diff new_branch...main
>
diff --git a/README.md b/README.md
index eca67e6..3eede6f 100644
Binary files a/README.md and b/README.md differ
diff --git a/test_1.c b/test_1.c
index ce0015b..796cc85 100644
--- a/test_1.c
+++ b/test_1.c
@@ -28,3 +28,13 @@ int some_new_func()
 {
   return 1234;
 }
+
+float return_float()
+{
+  return 1.0;
+}
+
+int new_func_3()
+{
+  return 123;
+}
```

### Merging Workflows

Самый простой сценарий это держать все в master ветке, и когда в других ветках что-то изменено, то просто сливать их в одну, но данная стратегия не подходит для более больших проектов. Другая стратегия подразумевает двойную проверку. Делается 2 ветки, master содержит в себе полностью протестированный код на постоянно обновляющейся главной ветке develop. И только когда все уверены в его надежности, происходит слияние master и develop.

### Если сделал всего один коммит в какой-то ветке и хочешь объединить объединить со старой (cherry-pick)

После этого можно удалить временную ветку и история коммитов будет чистой.

```console
git cherry-pick e43a6
```

### Включить функцию автоматического исправления merge конфликтов

Данная команда запоминает прошлые конфликты, и если новые похожи на старые она старается их решить.

```console
git config --global rerere.enabled true
```

### Показать версию сборки ветки согласно тегам

```console
git describe main
>v0.1-12-gbfad912
```

### Завернуть весь код в zip и подготовить к релизу

```console
git archive master --prefix='project/' --format=zip > `git describe master`.zip
```

### Показать историю коммитов сгруппированных по авторам, кто их делал

```console
git shortlog --no-merges main
```

---

## Reset Demystified

- tags

---

### Отменить прошлый коммит, но оставить рабочую дирректорию неизменной

```console
git reset --soft HEAD~
```

### Отменить прошлый коммит и отменить команду git add

```console
git reset --mixed HEAD~
```

### Отменить прошлый коммит, отменить команду git add и вернуть файлы в рабочей дирректории в состояние этого коммита

**hard является очень опасной командой и может повлечь за собой потерю данных, которые нельзя никак будет вернуть**

```console
git reset --hard HEAD~
```

---

## Revision Selection

- tags

---

Git позволяет ссылаться на один и тот же коммит несколькими путями.

### Single Revisions

Можно сслылаться на один единственный коммит, который выражается 40-character SHA-1 hash.

### Short SHA-1

В Git никакая запись в базе данных не начинается с одинакового префикса из 7 символов

### Показать коммиты с короткими хешами

```console
git log --abbrev-commit
```

### Branch References

Можно ссылаться не только на сами коммиты, но и на ветки.

### Показать последний коммит в данной ветке

```console
git show branch_name
```

### Показать хеш последнего коммита в данной ветке

```console
git rev-parse branch_name
```

### Показать последние операции с HEAD за последние несколько месяцев (reflog)

```console
git reflog
>
ea67c7d (HEAD -> main, custom_origin/main) HEAD@{0}: commit: maintain project
bfad912 HEAD@{1}: checkout: moving from main to main
bfad912 HEAD@{2}: cherry-pick: some body was
9d963e0 HEAD@{3}: checkout: moving from new_branch to main
58a305b (new_branch) HEAD@{4}: commit: some body was
9d963e0 HEAD@{5}: merge main: Fast-forward
e28a87f HEAD@{6}: reset: moving to HEAD
e28a87f HEAD@{7}: reset: moving to HEAD
e28a87f HEAD@{8}: checkout: moving from main to new_branch
9d963e0 HEAD@{9}: checkout: moving from new_branch to main
...
...
```

### Показать что было с репозиторием 5 шагов назад от текущего

```console
git show 'HEAD@{5}'
```

### Показать что было с репозиторием вчера

```console
git show 'branch_name@{yesterday}'
```

### Показать reflog как git log

```console
git log -g branch_name
```

### Ancestry References

Коммит также можно идентифицировать через предка.

### Показать предыдущий коммит, с указанного

```console
git log --pretty=format:'%h %s' --graph
>
* ea67c7d maintain project
* bfad912 some body was
* 9d963e0 new func 3
* 01929d6 distributed git
* d44be5c about rebase
* e28a87f some_new_func_2

git show HEAD^
>
commit bfad91216a67db89696c34c72c4a4294fc4d1cad
Author: dim web
```

### Показать предыдущий коммит n-го предка назад

```console
git show commit_hash~n_steps
```

### Commit Ranges

Можно указывать не только единичные коммиты, но и их промежутки.

### Показать различные коммиты веток, после того как они разошлись

```console
git log имя_ветки_с_которой_нужно_сравнить..имя_ветки_с_которой_сравнивают
```

---

## Interactive Staging

- tags

---

Можно коммитить проделанные изменения в один большой коммит, но это не будет корректно отражать его изменения, поэтому можно сделать несколько маленьких коммитов.

### Открыть интерактивное меню для добавление файлов в staged

В этой программе можно частично выбирать некоторые файлы, в более удобном режиме

```console
git add -i
```

---

## Stashing and Cleaning

- tags

---

Работая над проектами, ты постоянно переключаешься между ветками. И не всегда бывает так что ты успеваешь что-то доделать, ты не хочешь делать неполный коммит, но работу сохранить хочется. Для этого применяется механизм stashing, он берет временной состояние дирректории и сохраняет его.

### Сохранить свои временные изменения, без коммита (stash)

```console
git stash
```

### Показать весь список stash

```console
git stash list
```

### Удалить какой-то stash

```console
git stash drop stash_name
```

### Создать новую ветку с текущего stash

```console
git stash branch new_branch_name
```

### Удалить неотслеживаемые файлы

```console
git clean -d -n
```

---

## Searching

- tags

---

Git обладает встроенной функцией поиска, по типу grep из стандартного софта linux. Этот инструмент поможет найти функцию или переменную.

### Найти текст int во всех файлах текущей ветки

```console
git grep -n some_text
```

### Посчитать сколько текст встречается в каждом отдельном файле

```console
git grep --count some_text
```

### Поиск текста по коммитам

```console
git log -S some_text --oneline
```

---

## Rewriting History

- tags

---

### Изменить сообщенеи в последнем коммите

```console
git commit --amend
```

### Удалить какой-то файл во всех коммитах

```console
git filter-branch --tree-filter 'rm -f some_file' HEAD
```

---

## Advanced Merging

- tags

---

### Отменить только что произошедший merge, если вдруг возникла ошибка

```console
git merge --abort
```

### Если при merge возникают ошибки связанные с пробелами

```console
git merge -Xignore-space-change merge_branch
```

---

## Debugging with Git

- tags

---

### Показать файл построчно изменения по коммитам

```console
git blame -L line_num_start,line_num_end file_name
```

---

## Submodules

- tags

---

Вероятно нужно работать над отдельными проектами в едином репозитории. Для этого есть submodules

### Добавить репозиторий на сервере в существующий репозиторий

```console
git submodule add https://github.com/dmitrymailk/ReactCourse
```

### Инициализировать submodule в новой папке

```console
git submodule init
```

### Получить данные submodule с сервера

```console
git submodule update
```

### Клонировать репозиторий со всеми submodules

```console
git clone --recurse-submodules repo_url
```

### Обновить все submodules автоматически (это значит что обновится с родительского ресурса)

```console
git submodule update --remote name_submodule_repo
```

### Обновить все submodules автоматически (это значит что обновится с репозитория которые содержат родительские)

```console
git submodule update --init --recursive
```

---
