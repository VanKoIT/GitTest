### Инициализация git репозитория.
Cоздать пустой проект с git репозиторием. Установить в конфиге git имя и
email.
~~~
git init
git config user.name "Ivan Korepanov"
git config user.email "korepanov@dev.picom.ru"
~~~

### Создание коммитов
Добавить файл index.html, содержащий пустой html документ. Закоммитить.
Создать новый файл script.js и подключить его в index.html. Закоммитить
~~~
git add index.html
git commit -m "Add index.html"
git add .
git commit -m "Add script.js"
~~~

### Удаление файлов
Добавить фавикон. Добавить файл favicon.png, подключить его в index.html.
Закоммитить. Удалить фавикон, закоммитить
~~~
git add .
git commit -m "Add favicon.ico"
git add .
git commit -m "Remove favicon.ico"
~~~

### Откат последних изменений
Добавить строчку, откатить изменения
Добавить строчку, положить изменение в индекс, откатить изменение
Добавить строчку, закоммитить, откатить изменения

~~~
git checkout index.html

git add .
git reset HEAD index.html
git checkout index.html

git add .
git commit -m "Update index.html"
git revert HEAD --no-edit
~~~

### Состояние файлов.
Добавить файл test. Закоммитить. Удалить файл test. Добавить файл style.css
и подключить его в index.html. Добавить в индекс изменения в файле
index.html. Вывести состояние файлов с помощью команды status. Пояснить
вывод команды. Вывести состояния фалов в сокращенном формате. Откатить
изменения, выполненные в этом упражнении
~~~
git add .
git commit -m "Add test.txt"
git add index.html

git status
    On branch master
    //проиндексированные изменения
    Changes to be committed:
      (use "git restore --staged <file>..." to unstage)
            modified:   index.html //файл изменен
    //неотслеживаемые файлы
    Untracked files:
      (use "git add <file>..." to include in what will be committed)
            style.css

git status -s
    //M -файл изменен, ??-не отслеживается
    M  index.html
    ?? style.css               

//откат
git reset HEAD
git revert HEAD --no-edit
git clean -f 
git checkout index.html
~~~

### Внесение изменений в последний коммит.
Добавить файл style.css и подключить его в index.html. Создать коммит,
намеренно забыв добавить новый файл style.css в индекс. Изменить
описание последнего коммита и добавить в него забытый файл. Что
произойдет, если разработчик успел опубликовать коммит до внесения
исправлений? Можно ли вносить изменения в опубликованные коммиты?
~~~
git add index.html
git commit -m "Add style.css"
git add .
git commit --amend -m "Add style.css and include to index.html"
~~~
---
Если коммит был опубликован и подтянут остальными разработчикам, то им будет необходимо обновить ветку на своем компьютере.
Если у них есть какие-то новые коммиты, предком которых является удаляемый коммит, им придётся делать rebase на коммит.

Вносить изменения в опубликованные коммиты запрещено.
### Исключения файлов
Добавить файл log и исключить его из версионного контроля.
Добавить папку cache и исключить ее и все ее содержимое из версионного
контроля.
Добавить папку config и добавить ее в версионный контроль.
Закоммитить
---
Создаем файл .gitignore в корне проекта со следующим содержанием
~~~
log.txt
/cache
~~~
В папке config создаем файл .gitkeep. Делаем коммит:
~~~
git add .
git co
~~~

### Исключение из версионного контроля отслеживаемого файла
Добавить файл passwords. Закомитить. Удалить файл passwords из версионного контроля, не удаляя сам файл. Закоммитить изменения. Что произойдет с файлом passwords в другом репозитории, если там применить
этот коммит?
~~~
git add .
git commit -m "Add password.txt"
//добавляем password.txt в .gitignore
git add .
git commit -m "Exclude password.txt from tracking"
~~~

### Ветвление
Создать ветку include_jquery и в этой ветке подключить jquery. Создать ветку include_angular (от ветки master) и подключить в этой ветке angular. Посмотреть список веток. Переключиться между ветками
~~~
git checkout -b include_jquery
git add .
git commit -m "Add jquery"
git checkout -b include_angular
git add .
git commit -m "Add angular"
git checkout include_jquery
~~~

### Слияние
Слить ветку include_jquery в ветку master. Что означает Fast-forward
(перемотка)? Что такое трех-ходовое (3-way) слияние? В каких случаях
применяется перемотка, а в каких 3-х ходовое слияние. Можно ли
одновременно слить несколько веток?
~~~
git checkout master
git merge include_jquery
~~~
---
Fast-forward-Это просто один из вариантов выполнения операции merge. Он возможен только в том случае, если текущий коммит, является предком «сливаемого» коммита . В этом случае новая фиксация не требуется для хранения объединенной истории; вместо этого HEAD(вместе с индексом) обновляется, чтобы указать на названный коммит, без создания дополнительного коммита слияния.

Однако выполнить ускоренное слияние не получится, если ветки после разделения развивались независимо друг от друга. Если до целевой ветки нет прямого пути, Git будет вынужден объединить их методом трехстороннего слияния. Такое слияние выполняется с помощью специального коммита, который служит для объединения двух историй. Метод называется трехсторонним, поскольку Git использует три коммита для создания коммита слияния (последние коммиты двух веток и общий родительский элемент).

### Слияние с конфликтами
Слить ветку include_angular в ветку master. Разрешить конфликты слияния
~~~
git checkout master
git merge include_jquery
git add .
git commit -m "Merge conflict resolved"
~~~

### Откат непоследнего коммита
Добавить в index.html футер, закоммитить. Добавить в футер ссылку на разработчика, закоммитить. Добавить шапку, закоммитить. С помощью комманды revert откатить коммит, добавляющий ссылку на разработчика. Назвать другие способы отката.

~~~
git add .
git commit -m "Add footer to index.html"
git add .
git commit -m "Add link to developer to index.html"
git add .
git commit -m "Add header ti index.html"

git revert 588a927ccd0f59c610869922467b2306aea15505 --no-edit      
    hint: after resolving the conflicts, mark the corrected paths
    hint: with 'git add <paths>' or 'git rm <paths>'
    hint: and commit the result with 'git commit'     

//решаем конфликты перед продолжением
git add .       
git revert --continue     
~~~
---
Также можно сделать откат через reset

### История коммитов
Посмотреть историю коммитов в различных форматах, с применением
различных фильтров и сортировок.
~~~
git log
git log --graph
git log --oneline
git log --stat
git log --shortstat
git log --name-status
git log --relative-date
git log --abbrev-commit
~~~

### Diff
Посмотреть различия между рабочей дирректорией, индексом и последним
коммитом. Посмотреть различия между любыми коммитами и ветками
Посмотреть отличия различных коммитов и веток друг от друга.
Использовать команду diff c опцией --stat и без нее.
~~~
git diff HEAD
git diff --cached
git diff 2ff9f798c2d55de5983228101d76be5ca8b5bde9
git diff --stat 2ff9f798c2d55de5983228101d76be5ca8b5bde9
~~~
### Публикация репозитория, веток, коммитов

Опубликовать репозиторий на любом хостинге git (bitbucket.org, github.com).
Создать ветку, внести любое изменение в проект, опубликовать новую ветку.
Создать еще несколько коммитов и опубликовать их.
~~~
git remote add origin https://github.com/VanKoIT/GitTest.git
git branch -M main
git push -u origin main
~~~