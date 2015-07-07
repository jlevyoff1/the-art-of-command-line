[ На других языках: [English](README.md), [Español](README-es.md), [Português](README-pt.md), [中文](README-zh.md) ]

# Искусство командной строки

[![Вступайте в англоязычный чат проекта https://gitter.im/jlevy/the-art-of-command-line](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/jlevy/the-art-of-command-line?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

- [Описание](#meta)
- [Основы](#basics)
- [Ежедневное использование](#everyday-use)
- [Процессинг файлов и информации](#processing-files-and-data)
- [Системный дебаггинг](#system-debugging)
- [Команды в одну строчку](#one-liners)
- [Сложно, но полезно](#obscure-but-useful)
- [MacOS only](#macos-only)
- [Больше информации по теме](#more-resources)
- [Дисклеймер](#disclaimer)


![curl -s 'https://raw.githubusercontent.com/jlevy/the-art-of-command-line/master/README.md' | egrep -o '`\w+`' | tr -d '`' | cowsay -W50](cowsay.png)

Продвинутому использованию командной строки зачастую не уделяют достаточного внимания, о терминале говорят как о чем-то мистическом; на самом же деле это умение очевидно и не очевидно увеличивает Вашу продуктивность в работе. Данный документ является подборкой заметок и советов, которые я нашел для себя полезными, работая с командной строкой в Linux. Некоторые из их них – простые и очевидные, но некоторые довольно сложные и предназначены для решения конкретных задач. Это небольшая публикация, но если Вы знаете обо всем, что тут написано, и можете вспомнить как это все использовать – вы много знаете!

Многое из того, что тут написано
[изначально](http://www.quora.com/What-are-some-lesser-known-but-useful-Unix-commands)
[появилось](http://www.quora.com/What-are-the-most-useful-Swiss-army-knife-one-liners-on-Unix)
на [Quora](http://www.quora.com/What-are-some-time-saving-tips-that-every-Linux-user-should-know),
начав идею там, похоже, что стоит развить ее на Gihub, где обитают люди, которые талантлевее меня и могут предлагать улучения данной подборки. Если Вы заметили ошибки (во всех вариантах перевода), пожалуйста оставьте тикет или киньте пулл-реквест (заранее изучив описание и посмотрев на уже созданнные тикеты и пулл-реквесты).

## Описание

Основное:

- Данная публикация предназначена как для новичков, так и для опытных людей. Цели: *объемность* (собрать все важные аспекты использования командной строки), *практичность* (давать конкретные примеры для самых частых юзкейсов) и *краткость* (не стоит углубляться в неочевидные вещи, о которых можно почитать в другом месте).
- Этот документ написан для пользователей Linux, с единственным исключеним – секцией "[MacOS only](#macos-only)". Все остальное подходит и может быть установлено под все UNIX/MacOS системы (и даже Cygwin).
- Фокусируемся на интерактивном Баше, но многие вещи так же могут быть использованы с другими шеллами; и в общем приминимы к Баш-скирптингу
- Эта инструкция включает в себя стандартные Unix комманды и те, для которых нужно устанавливать сторонние пакеты – они настолько полезны, что стоят того, чтобы их установили

Заметки:

- Для того, чтобы руководство оставалось одностраничным, вся информация вставлена прямо сюда. Вы достаточно умные для того, чтобы самостоятельно изучить вопрос более детально в другом месте. Используйте `apt-get`/`yum`/`dnf`/`pacman`/`pip`/`brew` (в зависимости от вашей системы управления пакетами) для установки новых програм.

- На [Explainshell](http://explainshell.com/) можно найти простое и детальное объясение того, что такое команды, флаги, пайпы и т.д.

## Основы

- Выучите основы Баша. Просто возьмите и напечатайте `man bash` в терминале и хотя бы просмотрите его; он довольно просто читается и он не очень большой. Другие шеллы тоже могут быть хороши, но Баш – мощная программа и Баш всегда под рукой (использование *исключительно* zsh, fish и т.д., которые наверняка круто выглядят на Вашем ноуте во многом Вас ограничивает, например Вы не сможете использовать возможности этих шеллов на уже существующем сервере).

- Выучите хотя бы один консольный редактор текста. Идеально Vim (`vi`), ведь у него нет конкурентов, когда вам нужно быстренько что-то подправить (даже если Вы постоянно сидите на Emacs/какой-нибудь тяжелой IDE или на каком-нибудь модном хипстерском редакторе)

- Знайте как читать документацию через `man` (для любознательных – `man man`; `man` по углам документа в скобках добавляет номер, например 1 – для обычных команд, 5 – для файлов, конвенций, 8 – для администативных команд). Ищите мануалы через `apropos`, и помните, что некоторые команды – не бинарники, а встроенные команды Баша, и помощь по ним можно получить через `help` и `help -d`.

- Узнайте о том, как перенаправлять ввод и вывод через `>` и `<` и пайпы `|`. Помните, что `>` – переписывает выходной файл, а `>>` добавляет к нему. Узнайте побольше про stdout and stderr.

- Узнайте побольше про file glob expansion with `*` (and perhaps `?` and `{`...`}`), кавычки а так же разницу между двойными `"` и одинарными `'` кавычками. (Больше о расширении переменных читайте ниже)

- Будьте знакомы с работой с процессами в Bash: `&`, **ctrl-z**, **ctrl-c**, `jobs`, `fg`, `bg`, `kill`, и т.д.

- Знайте `ssh`, и основы безпарольной аунтефикации через `ssh-agent`, `ssh-add`, и т.д.

- Основы работы с файлами: `ls` и `ls -l` (в частности узнайте, что значит каждый столбец в  `ls -l`), `less`, `head`, `tail` и `tail -f` (или даже лучше, `less +F`), `ln` и `ln -s` (узнайте разницу между символьными ссылками и жесткими ссылками и почему жесткие ссылки лучше), `chown`, `chmod`, `du` (для быстрой сводки по использованию диска: `du -hk *`). Для менеджмента файловой системы, `df`, `mount`, `fdisk`, `mkfs`, `lsblk`.

- Основы работы с сетью: `ip` или `ifconfig`, `dig`.

- Хорошо знайте регулярные выражения и разные флаги к `grep`/`egrep`. Такие флаги как `-i`, `-o`, `-A`, и `-B` стоит знать.

- Обучитесь использованию системами управления пакетами `apt-get`, `yum`, `dnf` или `pacman` (в зависимости от дистрибутива). Занйте как искать и устанавливать пакеты и обязательно имейте установленым `pip` для установки командных утилит, написаных на Python (некоторые из тех, что вы найдете ниже легче всего установить через `pip`)


## Ежедневное использование

- Используйте таб в Баше для автокомплита аргументов к командам и **ctrl-r** для поиска по истории командной строки

- Используйте **ctrl-w** в Баше для того, чтобы удалить последнее слово в команде; **ctrl-u** для того, что бы удалить команду полностью. Используйте **alt-b** и **alt-f** для того, чтобы бегать между словами команды, **ctrl-k** для того, чтобы прыгнуть к концу строки, **ctrl-l** для того, чтобы очистить экран. Гляньте на `man readline` чтобы узнать о всех шорткатах Баша. Их много! Например, **alt-.** бежит по предыдущим аргументам команды, а **alt-*** расширяет глоб.??

– Если Вам нравятся шорткаты Вима сделайте `set -o vi`.

- Для того, чтобы посмотреть историю введите `history`. Так же существует множество аббривиатур, например `!$` – последний аргумент, `!!` – последняя команда, хотя эти аббривиатуры часто заменяются шорткатами **ctrl-r** и **alt-.**.

- Для того, чтобы прыгнуть к последней рабочей директории – `cd -`

- Если Вы написали команду наполовину и вдруг передумали нажмите **alt-#** для того, чтобы добавить `#` к началу и отправьте комманду как комментарий. Потом вы сможете вернуться к ней через историю.

- Не забывайте использовать `xargs` (или `parallel`). Это очень мощная штука. Обратите внимание, что Вы можете контролировать количество команд на каждую строку, а так же параллельность. Если Вы не уверены, что делаете что-то правильно, начните с `xargs echo`. Еще `-I{}` – полезная штука. Примеры:
```bash
      find . -name '*.py' | xargs grep some_function
      cat hosts | xargs -I{} ssh root@{} hostname
```

- `pstree -p` – полезный тип вывода дерева процессов.

- Используйте `pgrep` или `pkill` для того чтобы находить/слать сигналы к процессам по имени (`-f` помогает).

- Знайте разные сигналы, которые можно слать процессам. Например, чтобы приостановить процесс используйте `kill -STOP [pid]`. Для полного списка посмотрите `man 7 signal`.

- Используйте `nohup` или `disown` для того, чтобы процесс в фоне выполнялся бесконечно.

- Узнайте какие процессы слушают порты через `netstat -lntp` или `ss -plat` (для TCP; добавьте `-u` для UDP).

- Так же `lsof` для того, чтобы посмотреть открытые сокеты и файлы.

- Используйте `alias` для того, чтобы алиасить часто используемые команды. Например, `alias ll='ls -latr'` создаст новый алиас `ll`.

- В Баш скритах используйте `set -x` для того, чтобы дебажить аутпут. Используйте строгие режимы везде, где возможно. Используйте `set -e` для того, чтобы прекращать выполнение при ошибках. Используйте `set -o pipefail` для того, чтобы строго относится к ошибкам (это немного глубокая тема). Для более сложных скриптов так же используйте `trap`.

- В Баш скриптах, подоболочки (subshells) – удобный способ группировать команды. Один из самых распространенных примеров – временно передвинуться в другую рабочую директорию, вот так:
```bash
      # do something in current dir
      (cd /some/other/dir && other-command)
      # continue in original dir
```

- В Баше много типов пространства переменных. Проверить, существует ли переменная – `${name:?error message}`. Например, если Баш-скрипту нужен всего один аргумент просто напишите `input_file=${1:?usage: $0 input_file}`. Арифметическая область видимости: `i=$(( (i + 1) % 5 ))`. Последовательности: `{1..10}`. Обрезка строк: `${var%suffix}` и `${var#prefix}`. Например, если `var=foo.pdf` тогда `echo ${var%.pdf}.txt` выведет `foo.txt`.

- Вывод любой команды можно обратить в файл через `<(some command)`. Например, сравнение локального файла `/etc/hosts с удаленным:
```sh
      diff /etc/hosts <(ssh somehost cat /etc/hosts)
```

- Знайте про heredoc-синтаксис в Баше, работает он вот так `cat <<EOF ...`.

- В Баше перенаправляйте стандартный вывод, а так же стандартные ошибки вот так: `some-command >logfile 2>&1`. Зачастую для того, чтобы убедится, что команда не оставит открытым файл, привязав его к открытому терминалу считается хорошей практикой добавлять `</dev/null`.

- Используйте `man ascii` для хорошей ASCII таблицы, с hex и десятичными значениями. Для информации по основным кодировкам полезны: `man unicode`, `man utf-8`, и `man latin1`.

- Используйте `screen` или [`tmux`](https://tmux.github.io/) для того, чтобы иметь несколько экранов в одном терминале, это особенно полезно, когда вы работаете с удаленным сервером, тогда Вы можете подключаться/отключаться от сессий. Более минималистичный подход для этого – использование `dtach`.

- В SSH полезно знать как port tunnel с флагами `-L` и `-D` (и иногда `-R`), например для того, чтобы зайти на сайт с удаленного сервера.

- Еще может быть полезно оптимизировать вашу SSH конфигурацию, например этот файл `~/.ssh/config` содержит настройки, которые помогают избегать потерянные подключения в некоторых сетевых окружениях, используйте сжатие (которое полезно с scp через медленные подключения) и увеличьте количество каналов к одному серверу через этот конфиг вот так:
```
      TCPKeepAlive=yes
      ServerAliveInterval=15
      ServerAliveCountMax=6
      Compression=yes
      ControlMaster auto
      ControlPath /tmp/%r@%h:%p
      ControlPersist yes
```

- Некоторые другие настройки SSH могут сильно повлиять на безопасность и должны меняться осторожно, например для конкретной подсети или конкретной машины или в домашних сетях: `StrictHostKeyChecking=no`, `ForwardAgent=yes`

- Для того, чтобы получить разрешения файла в восьмеричном виде, что полезно для конфигурации систем, но нельзя получить из `ls`, можно использовать что-то типа:
```sh
      stat -c '%A %a %n' /etc/timezone
```

- Для интерактивного выделения результатов других команд используйте [`percol`](https://github.com/mooz/percol) or [`fzf`](https://github.com/junegunn/fzf).

- Для работы с файлами, список которых дала другая команда (например Git) используйте `fpp` ([PathPicker](https://github.com/facebook/PathPicker)).

- Для того чтобы быстро поднять веб-сервер в текущей директории (и поддерикториях), который доступен для всех в вашей сети используйте:
`python -m SimpleHTTPServer 7777` (for port 7777 and Python 2) and `python -m http.server 7777` (for port 7777 and Python 3).

- Для того, чтобы выполнить определенную команду с привилегиями, используйте `sudo` (для рута) и `sudo -u` (для другого пользователя). Используйте `su` или `sudo bash` для того чтобы запустить шелл от имени этого пользователя. Используйте `su -` для того, чтобы симулировать свежий логин от рута или другого пользователя.


## Процессинг файлов и информации

- Для того, чтобы найти файл в текущей директории сделайте `find . -iname '*something*'`. Для того, чтобы искать файл по всей системе используйте `locate something` (но не забывайте, что `updatedb` мог еще не проидексировать недавно созданные файлы).

- Для основого поиска по содержимому файлов (более сложному, чем `grep -r`) используйте [`ag`](https://github.com/ggreer/the_silver_searcher).

- Для конвертации HTML в текст: `lynx -dump -stdin`

- Для конвертации разных типов разметки (HTML, маркдаун и т.д.) попробуйте [`pandoc`](http://pandoc.org/).

- Если нужно работать с XML, есть старая но хорошая утилита – `xmlstarlet`.

- Для работы с JSON используйте `jq`.

- Для работы с Excel и CSV-файлами используйте [csvkit](https://github.com/onyxfish/csvkit) (программа предоставляет команды `in2csv`, `csvcut`, `csvjoin`, `csvgrep` и т.д.)

- Для работы с Amazon S3 удобно работать с [`s3cmd`](https://github.com/s3tools/s3cmd) и [`s4cmd`](https://github.com/bloomreach/s4cmd) (последний работает быстрее). Для остальных сервисов Амазона используйте стандартный [`aws`](https://github.com/aws/aws-cli).

- Знайте про `sort` и `uniq`, включая флаги `-u` и `-d`, смотрите примеры ниже. Еще гляньте на `comm`.

- Знайте про `cut`, `paste`, и `join` для работы с текстовыми файлами. Многие люди используют `cut` забыв про `join`.

- Знайте о `wc`: для подсчета переводов строк (`-l`), для символов – (`-m`), для слов – words (`-w`), для байтового подсчета – (`-c`).

- Знайте про `tee`, для копирования в файл из stdin и stdout, что-то типа `ls -al | tee file.txt`.

- Не забывайте, что Ваша локаль влияет на многие команды, включая порядки сортировки, сравнение и производительность. Многие дистрибутивы Linux автоматически выставляют `LANG` или любую другую переменную в подходящую для Вашего региона. Из-за этого результаты функций сортировки могут работать непредсказуемо. Рутины `i18n` могут значительно снизить производительность сортировок. В некоторых случаях можно полностью этого избегать (за исключением редких случаев), сортируя традиционно побайтово, для этого `export LC_ALL=C`

- Знайте основы `awk` и `sed` для простых манипуляций с данными. Например, чтобы получить сумму всех чисел, которые находятся в третьей колонки текстового файла можно использовать `awk '{ x += $3 } END { print x }'`. Скорее всего, это раза в 3 быстрее и раза в 3 проще чем делать это в Питоне.

- Чтобы заменить все нахождения подстроки в одном или нескольких файлах:
```sh
      perl -pi.bak -e 's/old-string/new-string/g' my-files-*.txt
```

Для того, чтобы переименовать сразу много файлов по шаблону, используйте  `rename`. Для сложных переименований может помочь [`repren`](https://github.com/jlevy/repren):

```sh
      # Восстановить бекапы из foo.bak в foo:
      rename 's/\.bak$//' *.bak
      # Полное переименование имен файлов, папок и содержимого файлов из foo в bar.
      repren --full --preserve-case --from foo --to bar .
```

- Используйте `shuf` для того, чтобы перемешать строки или выбрать случайную строчку из файла.

- Знайте флаги `sort`а. Для чисел используйте `-n`, для работы с человекочитаемыми числами используйте `-h` (например `du -h`). Знайте как работают ключи (`-t` и `-k`). В частности, не забывайте что вам нужно писать `-k1,1` для того, чтобы отсортировать только первое поле; `-k1` значит сортировка учитывая всю строчку. Так же стабильная сортировка может быть полезной (`sort -s`). Например для того, чтобы отсортировать самое важное по второму полю, а второстепенное по первому можно использовать sort -k1,1 | sort -s -k2,2`.

- Если вам когда-нибудь придется написать код таба в терминале, например для сортировки по табу с флагом -t, используйте шорткат **ctrl-v** **[Tab]** или напишите `$'\t'`. Последнее лучше, потому что его можно скопировать.

- Стандартные инструменты для патчинга исходников это `diff` и `patch`. Так же посмотрите на `diffstat` для просмотра статистики диффа. `diff -r` работает для по всей директории. Используйте `diff -r tree1 tree2 | diffstat` для полной сводки изменений.

- Для бинарников используйте `hd` для простых hex-дампом и `bvi` для двоичного изменения бинарников.

- `strings` (в связке `grep` или чем-то похожем) помогает найти строки в бинарниках.

- Для того, чтобы посмотреть разницу в бинарниках (дельта кодирование) используйте `xdelta3`.

- Для конвертирования кодировок используйте `iconv`. Для более сложных задач – `uconv`, он поддерживает некоторые сложные фичи Юникода. Например эта команда переводит строки из файла в нижний регистр и убирает ударения (кои бывают, например, в Испанском)

```sh
      uconv -f utf-8 -t utf-8 -x '::Any-Lower; ::Any-NFD; [:Nonspacing Mark:] >; ::Any-NFC; ' < input.txt > output.txt
```

- Для того, чтобы разбить файл на куски используйте `split` (разбивает на куски по размеру), или `csplit` (по шаблону или регулярному выражению)

- Используйте `zless`, `zmore`, `zcat`, и `zgrep` для работы с сжатыми файлами.

## System debugging

- For web debugging, `curl` and `curl -I` are handy, or their `wget` equivalents, or the more modern [`httpie`](https://github.com/jakubroztocil/httpie).

- To know disk/cpu/network status, use `iostat`, `netstat`, `top` (or the better `htop`), and (especially) `dstat`. Good for getting a quick idea of what's happening on a system.

- For a more in-depth system overview, use [`glances`](https://github.com/nicolargo/glances). It presents you with several system level statistics in one terminal window. Very helpful for quickly checking on various subsystems.

- To know memory status, run and understand the output of `free` and `vmstat`. In particular, be aware the "cached" value is memory held by the Linux kernel as file cache, so effectively counts toward the "free" value.

- Java system debugging is a different kettle of fish, but a simple trick on Oracle's and some other JVMs is that you can run `kill -3 <pid>` and a full stack trace and heap summary (including generational garbage collection details, which can be highly informative) will be dumped to stderr/logs.

- Use `mtr` as a better traceroute, to identify network issues.

- For looking at why a disk is full, `ncdu` saves time over the usual commands like `du -sh *`.

- To find which socket or process is using bandwidth, try `iftop` or `nethogs`.

- The `ab` tool (comes with Apache) is helpful for quick-and-dirty checking of web server performance. For more complex load testing, try `siege`.

- For more serious network debugging, `wireshark`, `tshark`, or `ngrep`.

- Know about `strace` and `ltrace`. These can be helpful if a program is failing, hanging, or crashing, and you don't know why, or if you want to get a general idea of performance. Note the profiling option (`-c`), and the ability to attach to a running process (`-p`).

- Know about `ldd` to check shared libraries etc.

- Know how to connect to a running process with `gdb` and get its stack traces.

- Use `/proc`. It's amazingly helpful sometimes when debugging live problems. Examples: `/proc/cpuinfo`, `/proc/xxx/cwd`, `/proc/xxx/exe`, `/proc/xxx/fd/`, `/proc/xxx/smaps`.

- When debugging why something went wrong in the past, `sar` can be very helpful. It shows historic statistics on CPU, memory, network, etc.

- For deeper systems and performance analyses, look at `stap` ([SystemTap](https://sourceware.org/systemtap/wiki)), [`perf`](http://en.wikipedia.org/wiki/Perf_(Linux)), and [`sysdig`](https://github.com/draios/sysdig).

- Check what OS you're on with `uname` or `uname -a` (general Unix/kernel info) or `lsb_release -a` (Linux distro info).

- Use `dmesg` whenever something's acting really funny (it could be hardware or driver issues).


## One-liners

A few examples of piecing together commands:

- It is remarkably helpful sometimes that you can do set intersection, union, and difference of text files via `sort`/`uniq`. Suppose `a` and `b` are text files that are already uniqued. This is fast, and works on files of arbitrary size, up to many gigabytes. (Sort is not limited by memory, though you may need to use the `-T` option if `/tmp` is on a small root partition.) See also the note about `LC_ALL` above and `sort`'s `-u` option (left out for clarity below).
```sh
      cat a b | sort | uniq > c   # c is a union b
      cat a b | sort | uniq -d > c   # c is a intersect b
      cat a b b | sort | uniq -u > c   # c is set difference a - b
```

- Use `grep . *` to visually examine all contents of all files in a directory, e.g. for directories filled with config settings, like `/sys`, `/proc`, `/etc`.


- Summing all numbers in the third column of a text file (this is probably 3X faster and 3X less code than equivalent Python):
```sh
      awk '{ x += $3 } END { print x }' myfile
```

- If want to see sizes/dates on a tree of files, this is like a recursive `ls -l` but is easier to read than `ls -lR`:
```sh
      find . -type f -ls
```

- Use `xargs` or `parallel` whenever you can. Note you can control how many items execute per line (`-L`) as well as parallelism (`-P`). If you're not sure if it'll do the right thing, use xargs echo first. Also, `-I{}` is handy. Examples:
```sh
      find . -name '*.py' | xargs grep some_function
      cat hosts | xargs -I{} ssh root@{} hostname
```

- Say you have a text file, like a web server log, and a certain value that appears on some lines, such as an `acct_id` parameter that is present in the URL. If you want a tally of how many requests for each `acct_id`:
```sh
      cat access.log | egrep -o 'acct_id=[0-9]+' | cut -d= -f2 | sort | uniq -c | sort -rn
```

- Run this function to get a random tip from this document (parses Markdown and extracts an item):
```sh
      function taocl() {
        curl -s https://raw.githubusercontent.com/jlevy/the-art-of-command-line/master/README.md |
          pandoc -f markdown -t html |
          xmlstarlet fo --html --dropdtd |
          xmlstarlet sel -t -v "(html/body/ul/li[count(p)>0])[$RANDOM mod last()+1]" |
          xmlstarlet unesc | fmt -80
      }
```


## Obscure but useful

- `expr`: perform arithmetic or boolean operations or evaluate regular expressions

- `m4`: simple macro processor

- `yes`: print a string a lot

- `cal`: nice calendar

- `env`: run a command (useful in scripts)

- `printenv`: print out environment variables (useful in debugging and scripts)

- `look`: find English words (or lines in a file) beginning with a string

- `cut `and `paste` and `join`: data manipulation

- `fmt`: format text paragraphs

- `pr`: format text into pages/columns

- `fold`: wrap lines of text

- `column`: format text into columns or tables

- `expand` and `unexpand`: convert between tabs and spaces

- `nl`: add line numbers

- `seq`: print numbers

- `bc`: calculator

- `factor`: factor integers

- `gpg`: encrypt and sign files

- `toe`: table of terminfo entries

- `nc`: network debugging and data transfer

- `socat`: socket relay and tcp port forwarder (similar to `netcat`)

- `slurm`: network trafic visualization

- `dd`: moving data between files or devices

- `file`: identify type of a file

- `tree`: display directories and subdirectories as a nesting tree; like `ls` but recursive

- `stat`: file info

- `tac`: print files in reverse

- `shuf`: random selection of lines from a file

- `comm`: compare sorted files line by line

- `pv`: monitor the progress of data through a pipe

- `hd` and `bvi`: dump or edit binary files

- `strings`: extract text from binary files

- `tr`: character translation or manipulation

- `iconv` or `uconv`: conversion for text encodings

- `split `and `csplit`: splitting files

- `sponge`: read all input before writing it, useful for reading from then writing to the same file, e.g., `grep -v something some-file | sponge some-file`

- `units`: unit conversions and calculations; converts furlongs per fortnight to twips per blink (see also `/usr/share/units/definitions.units`)

- `7z`: high-ratio file compression

- `ldd`: dynamic library info

- `nm`: symbols from object files

- `ab`: benchmarking web servers

- `strace`: system call debugging

- `mtr`: better traceroute for network debugging

- `cssh`: visual concurrent shell

- `rsync`: sync files and folders over SSH

- `wireshark` and `tshark`: packet capture and network debugging

- `ngrep`: grep for the network layer

- `host` and `dig`: DNS lookups

- `lsof`: process file descriptor and socket info

- `dstat`: useful system stats

- [`glances`](https://github.com/nicolargo/glances): high level, multi-subsystem overview

- `iostat`: CPU and disk usage stats

- `htop`: improved version of top

- `last`: login history

- `w`: who's logged on

- `id`: user/group identity info

- `sar`: historic system stats

- `iftop` or `nethogs`: network utilization by socket or process

- `ss`: socket statistics

- `dmesg`: boot and system error messages

- `hdparm`: SATA/ATA disk manipulation/performance

- `lsb_release`: Linux distribution info

- `lsblk`: List block devices: a tree view of your disks and disk paritions

- `lshw`, `lscpu`, `lspci`, `lsusb`, `dmidecode`: hardware information, including CPU, BIOS, RAID, graphics, devices, etc.

- `fortune`, `ddate`, and `sl`: um, well, it depends on whether you consider steam locomotives and Zippy quotations "useful"


## MacOS only

Некоторые вещи релевантны *только* для Мака.

- Системы управлением пакетами – `brew` (Homebrew) и `port` (MacPorts). Они могут быть использованы для того, чтобы установить большинство програм, упомянутых в этом документе.

- Копируйте аутпут консольных программ в десктопные через `pbcopy`, и вставляйте инпут через `pbpaste`.

- Для того, чтобы открыть файл или десктопную программу типа Finder используйте `open`, вот так `open -a /Applications/Whatever.app`.

- Spotlight: Ищите файлы в консоле через `mdfind` и смотрите метадату (например EXIF информацию фотографий) через `mdls`.

- Не забывайте, что MacOS основан на BSD Uni и многие команды (например `ps`, `ls`, `tail`, `awk`, `sed`) имеют некоторые небольшие различия с линуксовыми. Это обусловлено влянием `UNIX System V` и `GNU Tools`. Разницу можно заметить увидив заголовок "BSD General Commands Manual." к манам програм. В некоторых случаях, на Мак можно поставить GNU-версии программ, например `gawk` и `gsed`. Когда пишите кроссплатформенные Bash-скрипты, старайтесь избегать команды, которые могут различаться (например, лучше используйте Python или `perl`), или тщательно все тестируйте.

## Больше информации по теме

- [awesome-shell](https://github.com/alebcay/awesome-shell): Дополняемый список инструментов и ресурсов для командной строки.
- [Strict mode](http://redsymbol.net/articles/unofficial-bash-strict-mode/) Для того, чтобы писать шелл-скрипты лучше.


## Дисклеймер

За небольшим исключением, весь код написан так, что другие его смогут прочитать.

Кому много дано, с того много и спрашивается. Тот факт, что что-то может быть написано в Баше, вовсе не означает что оно должно быть там написано. ;)


## Лицензия

[![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)

Оригинальная работа и перевод на русский язык распространяется под лицензией [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).