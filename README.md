# Cамая удобная шпаргалка по работе с Git

## 1. Командная строка

### 1.1 Навигация в командной строке
**Домашняя директория** (англ. home directory) — каталог с файлами пользователя. Обычно здесь хранится такая информация, как загрузки, медиа, скриншоты и так далее.

`pwd` (от англ. print working directory — «показать рабочую папку») - выводит путь к текущей директории.

`cd` (от англ. change directory — «сменить директорию»).

`cd` и символ `~` — перейти к домашней директории.

Допустим, вы находитесь в директории `/projects`. Если ввести команду `cd github`, она перенесёт вас в директорию `/projects/github`.

Если в названии папки есть пробелы, при вводе нужно использовать кавычки.

```bash
$ cd "Фотографии с дня рождения"
```

Чтобы вернуться в **родительскую директорию** — то есть на уровень выше, — вместо названия папки нужно написать две точки: `..`.

```bash
$ pwd
/projects/github # сейчас мы здесь

$ cd .. # переходим на уровень выше

$ pwd
/projects # мы вернулись!
```

`ls` (от англ. list directory contents — «отобразить содержимое директории»).

`ls` с флагом `-a` выведет расширенный список. В нём отобразятся все скрытые файлы, которые начинаются с символа `.` (например, файлы конфигурации).  В том числе два особых файла `.` и `..`, которые обозначают текущую и родительскую директории.

```bash
$ ls # вывели список файлов
file.txt
photo.png

$ ls -a # вывели список, в котором отображаются скрытые файлы ., .. и .git
.
..
.git
file.txt
photo.png
```

### 1.2 Создание, копирование, перемещение

#### Создание файлов и директорий — touch, mkdir

`touch` (англ. «коснуться») с именем файла в качестве параметра - создание файла

```bash
$ touch my-new-file.txt # создали файл my-new-file.txt
```

`mkdir` (от англ. make directory — «создать директорию») - создание директории

```bash
$ mkdir new-dir # создали директорию new-dir
```

`mkdir` с флагом `-p` - создание структуры директорий

```bash
$ mkdir -p dir1/dir-inside/dir-deeper-inside
# создали папку dir-deeper-inside в папке dir-inside, которая находится в папке dir1
```

#### Копирование файлов — cp

`cp` (от англ. copy — «копировать») - копирования файлов

В простом виде `cp` принимает два параметра: `что копируем` и `куда копируем`.

```bash
$ cp index.html src/
# скопировали index.html в папку src
```

Но можно указать сразу несколько файлов.

```bash
$ cp что_копируем что_копируем что_копируем куда_копируем

$ cp index.html style.css script.js src/
# скопировали три файла (index.html, style.css и script.js) в папку src
```

#### Перемещение файлов и папок — mv

Синтаксис команды `mv` аналогичен синтаксису `cp`. После имени команды указывают список файлов и папок, которые нужно переместить, а затем — папку, в которую нужно выполнить перемещение.

```bash
$ mv table.csv ./very-important-files
# сначала указываем имя файла, который хотим переместить, потом путь — куда перемещаем 
```

### 1.3 Чтение и удаление

#### Чтение файлов — cat

`cat` (от англ. concatenate and print — «объединить и распечатать») - распечатывает файл в консоль. После команды нужно ввести имя файла.

Команда `cat` работает только с текстовыми файлами.

#### Удаление файлов и папок — rm, rmdir, rm -r

`rm` (от англ. remove — «удалить») - удаляет файл.

```bash
$ rm example.txt # удалили файл example.txt из текущей папки
```

`rmdir` (от англ. remove directory — «удалить директорию») - удаляет папку.

```bash
$ rmdir images # команда удалит папку images из текущей директории, 
               # если папка images пуста
```

Если в папке, которую вы пытаетесь стереть, есть какие-то файлы, то командная строка не удалит её и выведет сообщение о том, что папка не пуста (англ. `Directory not empty`).

`rm -r` (`-r` — от англ. recursive, «рекурсивный») рекурсивно удаляет файлы и папки.

Это значит, что удаление будет последовательно применяться к каждому из элементов в этой папке — пока не сотрёт их все. Затем команда удалит пустую директорию.

### 1.4 Работа с файлом настройки .gitconfig

Чтобы участникам проекта было понятно, кто и какие изменения вносил, нужно указать имя пользователя и адрес электронной почты.

`git config` (от англ. *configuration —* «конфигурация», «настройка») с ключом `--global` (англ. «глобальный»)

```bash
$ git config --global user.name "User Namovich" 
# имя или ник нужно написать латиницей и в кавычках

$ git config --global user.email username@yandex.ru
# здесь нужно указать свой настоящий email
```

Все глобальные настройки Git хранит в файле `.gitconfig` в домашней директории.

```bash
$ cat ~/.gitconfig
```

Другой способ проверки — вывести содержимое файла конфигурации Git той же командой `git config` с флагом `--list` (англ. «список»).

```bash
$ git config --list
```

## 2. Первый коммит

### 2.1 Инициализируем репозиторий

#### Сделать папку репозиторием — git init

Чтобы Git начал отслеживать изменения в проекте, папку с файлами этого проекта нужно сделать **Git-репозиторием** (от англ. *repository* — «хранилище»).  Для этого следует переместиться в неё и ввести команду `git init` (от англ. ***init**ialize* — «инициализировать»).

```bash
$ cd ~/dev/first-project # перешли в нужную папку

$ git init # создали репозиторий
```

#### «Разгитить» папку, если что-то пошло не так, — rm -rf .git

Если вы случайно сделали Git-репозиторием не ту папку, её можно «разгитить». Для этого нужно удалить скрытую подпапку `.git`.

```bash
$ cd <папка с репозиторием> # перешли в папку

$ rm -rf .git # удалили подпапку .git
```

Разберём подробнее, что такое `-rf`:

- ключ `r` (от англ. recursive — «рекурсивно») позволяет удалять папки вместе с их содержимым;
- ключ `f` (от англ. force — «заставить») избавит вас от вопросов вроде «Вы точно хотите удалить этот файл? А этот? И этот тоже?».

#### Проверить состояние репозитория — git status

`git status` (от англ. status — «статус», «состояние») — показывает текущее состояние репозитория.

### 2.2 Добавляем файлы в репозиторий

`git add --all` (от англ. add — «добавить» + от англ. all — «всё»). Ключ, или флаг, `--all` позволяет подготовить к сохранению все файлы в репозитории.

```bash
$ git add --all # подготовили к сохранению все файлы в репозитории
$ git status # проверили статус
```

Добавлять файлы можно и по одному, без ключа `--all`.

```bash
$ git add todo.txt
$ git add readme.txt
```

Также можно добавить текущую папку целиком — в этом случае все файлы в ней тоже будут добавлены. Обратиться к текущей папке в Bash позволяет точка (`.`).

```bash
$ git add . # добавить всю текущую папку
```

### 2.3 Делаем первый коммит

Сделать коммит можно командой `git commit` c ключом `-m` (от англ. message — «сообщение»), который присваивает коммиту сообщение.

Обычно в таком сообщении поясняется, в чём именно состояли изменения.

```bash
$ git commit -m 'Мой первый коммит!'
```

Команда `git commit` выведет информацию о коммите.

- `[master (root-commit) baa3b6e]` значит:
    - коммит был в ветке `master`;
    - `root-commit` — это самый первый, или «корневой» (англ. root), коммит в ветке, у следующих коммитов такой надписи не будет;
    - `baa3b6e` — сокращённый идентификатор коммита (подробнее об этом мы ещё расскажем).
- `2 files changed, 1 insertion(+)` значит:
    - изменились два файла (`readme.txt` и `todo.txt`);
    - одна строка была добавлена (`1. Пройти пару уроков по Git.`).
- Строки вида `create mode 100644 readme.txt` — это более подробная информация о новых (добавленных в Git) файлах.
    - `create` (англ. «создать») говорит, что файл был создан. Если бы файл был удалён, на этом месте было бы слово `delete` (англ. «удалить»).
    - `mode 100644` сообщает, что это обычный файл. Также возможны варианты `100755` для исполняемых файлов (например, `что-нибудь.exe`) и `120000` для файлов-ссылок в Linux. Файлы-ссылки не содержат данных сами по себе, а только ссылаются на другие файлы — как «ярлыки» в Windows.
	
### 2.4 Просматриваем историю коммитов

`git log` (от англ. *log* — «журнал») - показывет историю коммитов.

По умолчанию `git log` выводит коммиты в обратном хронологическом порядке — последние коммиты оказываются первыми сверху.

## 3. Синхронизация репозиториев

### 3.1 Что такое SSH. Генерируем SSH-ключ

#### Что такое SSH

**SSH** (от англ. Secure Shell Protocol) - один из наиболее распространённых сетевых протоколов.

Он обеспечивает безопасный обмен данными в сети. С помощью этого протокола можно получать данные с удалённого компьютера или отправлять их на него. Трафик шифруется, поэтому протокол безопасен.

SSH использует пару ключей для обеспечения безопасности — публичный и приватный:

- **Приватный ключ** (англ. *private key*) хранится только на вашем компьютере и не должен передаваться кому-либо ещё. Он используется для расшифровки данных.
- **Публичный ключ** (англ. *public key*) доступен всем и используется для шифрования данных. Они могут быть расшифрованы парным приватным ключом.

Только вы можете расшифровать данные с помощью приватного ключа, но любой владелец публичного ключа может их для вас зашифровать. Эти два ключа связаны и образуют **SSH-пару**.

#### Проверка наличия SSH-ключа

По умолчанию директория с SSH-ключами находится в домашней директории пользователя.

```bash
$ cd ~ # перешли в домашнюю директорию
$ ls -la .ssh/ # вывели список созданных ключей
```

Если папка пустая или её нет, всё в порядке.

Если есть файлы с похожими названиями, SSH-ключи уже создавались:

- `id_dsa.pub`;
- `id_ecdsa.pub`;
- `id_ed25519.pub`;
- `id_rsa.pub`.

#### Инструкция по генерации SSH-ключа

1. Для генерации SSH-пары можно использовать программу `ssh-keygen`. Откройте терминал и введите следующую команду.

```bash
$ ssh-keygen -t ed25519 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"
```

Если вы видите сообщение об ошибке, то, скорее всего, ваша система не поддерживает алгоритм шифрования `ed25519`. Ничего страшного: используйте другой алгоритм.

```bash
$ ssh-keygen -t rsa -b 4096 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"
```

После ввода отобразится такое сообщение.

```bash
> Generating public/private rsa key pair. # сгенерированы публичный и приватный ключи
```

2. Укажите место хранения ключей. Простой вариант — сделать домашний каталог пользователя путём по умолчанию. Для этого нажмите `Enter`.
3. Программа запросит **кодовую фразу** (англ. passphrase) для доступа к SSH-ключу. Вы можете оставить поле пустым. Для этого нажмите `Enter`, а затем ещё раз `Enter` для подтверждения.

```bash
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```

4. Теперь осталось проверить, что ключи действительно сгенерировались. Для этого вызовите эту команду.

```bash
ls -a ~/.ssh
```

На экране должны появиться два файла — один с расширением `.pub`, другой — без. Файл в `.pub` — публичный, им можно делиться с веб-сайтами или коллегами. Файл без расширения `.pub` — приватный. Ни в коем случае не передавайте его никому!

### 3.2 Привязываем SSH-ключ к GitHub

1. Скопируйте содержимое файла с публичным ключом в буфер обмена.

```bash
# скопировать содержимое ключа в буфер обмена:
$ clip < ~/.ssh/id_rsa.pub
# для ed25519:
$ clip < ~/.ssh/id_ed25519.pub
```

2. Перейдите на GitHub и выберите пункт **Settings.** В меню слева нажмите на пункт **SSH and GPG keys**. В открывшейся вкладке выберите **New SSH key.** В поле **Title** напишите название ключа. Например, **Personal key.** В поле **Key type** должно быть **Authentication Key**.
    
    В поле **Key** скопируйте ваш ключ из буфера обмена.
    
3. Проверьте правильность ключа с помощью следующей команды.

```bash
$ ssh -T git@github.com
```

### 3.3 Связываем локальный и удалённый репозитории

#### Привязать удалённый репозиторий к локальному — git remote add

Перейдите на страницу удалённого репозитория, выберите тип `SSH` и скопируйте URL.

Откройте консоль, перейдите в каталог локального репозитория и введите команду `git remote add` (от англ. *remote* — «удалённый» и *add* — «добавить»).

```bash
$ cd ~/dev/first-project
$ git remote add origin git@github.com:%ИМЯ_АККАУНТА%/first-project.git
```

Команде необходимо передать два параметра: имя удалённого репозитория и его URL. В качестве имени используйте слово `origin`. А URL вы скопировали со страницы удалённого репозитория.

`origin` (англ. «источник») — стандартный псевдоним, с помощью которого можно обращаться к главному удалённому репозиторию (обычно такой репозиторий один).

#### Убедиться, что репозитории связаны, — git remote -v

```bash
$ git remote -v
origin    git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (fetch)
origin    git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (push)
```

Флаг `-v` — короткая форма флага `--verbose` (англ. «подробный»). Он позволяет показать больше информации в выводе.

### 3.4 Синхронизируем локальный и удалённый репозитории

#### Отправить изменения на удалённый репозиторий — git push

`git push` (от англ. *push* — «толкать») - загружает содержимое локального репозитория на GitHub.

В первый раз эту команду нужно вызвать с флагом `-u` и параметрами `origin` (имя удалённого репозитория) и `main` или `master` (название текущей ветки). Флаг `-u` свяжет локальную ветку с одноимённой удалённой.

```bash
$ git push -u origin main # Если команда приведёт к ошибке, попробуйте 
                          # заменить main на master.
```

В дальнейшем при работе с удалённым репозиторием флаг `-u` можно опустить и писать просто `git push`.

## 4. Навигация по коммитам

### 4.1 Хеш — идентификатор коммита

#### Что такое хеш. Хеширование коммитов

**Хеширование** (от англ. *hash*, «рубить», «крошить», «мешанина») — это способ преобразовать набор данных и получить их «отпечаток» (англ. *fingerprint*).

Информация о коммите — это набор данных: когда был сделан коммит, содержимое файлов в репозитории на момент коммита и ссылка на предыдущий, или **родительский** (англ. *parent*), коммит.

Git хеширует (преобразует) информацию о коммите с помощью алгоритма **SHA-1** (от англ. Secure Hash Algorithm — «безопасный алгоритм хеширования») и получает для каждого коммита свой уникальный **хеш** — результат хеширования.

Обычно хеш — это короткая (4040 символов в случае SHA-1) строка, которая состоит из цифр 0— 9 и латинских букв *A*—*F* (неважно, заглавных или строчных). Она обладает следующими важными свойствами:

- если хеш получить дважды для одного и того же набора входных данных, то результат будет гарантированно одинаковый;
- если хоть что-то в исходных данных поменяется (хотя бы один символ), то хеш тоже изменится (причём сильно).

#### Хеш — основной идентификатор коммита

Git хранит таблицу соответствий `хеш → информация о коммите`. Если вы знаете хеш, вы можете узнать всё остальное: автора и дату коммита и содержимое закоммиченных файлов. Можно сказать, что хеш — основной идентификатор коммита.

Все хеши и таблицу `хеш → информация о коммите` Git сохраняет в служебные файлы. Они находятся в скрытой папке `.git` в репозитории проекта.