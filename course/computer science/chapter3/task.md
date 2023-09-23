﻿# Курс: Название курса
## Практическое задание №3. "Сценарии оболочки"

# Задания

1. Прочтите [`man ls`](https://www.man7.org/linux/man-pages/man1/ls.1.html) и напишите команду `ls`, которая выводит список файлов следующим образом.

     - Включает все файлы, включая скрытые файлы
     - Размеры указаны в удобочитаемом формате (например, 454M вместо 454279954).
     - Файлы упорядочены по давности.
     - Вывод раскрашен

     Пример вывода будет выглядеть так

    ```
    -rw-r--r--   1 user group 1.1M Jan 14 09:53 baz
    drwxr-xr-x   5 user group  160 Jan 14 09:53 .
    -rw-r--r--   1 user group  514 Jan 14 06:42 bar
    -rw-r--r--   1 user group 106M Jan 13 12:12 foo
    drwx------+ 47 user group 1.5K Jan 12 18:08 ..
    ```

--------
2. Напишите функции bash marco и polo, которые делают следующее.
Всякий раз, когда вы выполняете `marco`, текущий рабочий каталог должен каким-то образом сохраняться, затем, когда вы выполняете `polo`, независимо от того, в каком каталоге вы находитесь, `polo` должен `cd` вернуться в каталог, в котором вы выполнили `marco`.
Для упрощения отладки вы можете записать код в файл `marco.sh` и (повторно) загрузить определения в свою оболочку, выполнив `source marco.sh`.

--------
3. Допустим, у вас есть команда, которая редко дает сбой. Чтобы отладить его, вам необходимо записать его выходные данные, но запуск сбоя может занять много времени.
Напишите сценарий bash, который запускает следующий сценарий до тех пор, пока он не завершится сбоем, записывает стандартный вывод и потоки ошибок в файлы и печатает все в конце.
Бонусные баллы, если вы также сможете сообщить, сколько запусков потребовалось для сбоя сценария.

    ```bash
    #!/usr/bin/env bash

    n=$(( RANDOM % 100 ))

    if [[ n -eq 42 ]]; then
       echo "Something went wrong"
       >&2 echo "The error was using magic numbers"
       exit 1
    fi

    echo "Everything went according to plan"
    ```

--------

4. Как мы уже говорили, `-exec` команды `find` может быть очень мощным инструментом для выполнения операций над файлами, которые мы ищем.
Однако что, если мы захотим что-то сделать со **всеми** файлами, например создать zip-файл?
Как вы видели до сих пор, команды будут принимать входные данные как из аргументов, так и из `STDIN`.
При передаче команд мы подключаем `STDOUT` к `STDIN`, но некоторые команды, такие как tar, принимают входные данные из аргументов.
Чтобы преодолеть это отключение, существует команда [`xargs`](https://www.man7.org/linux/man-pages/man1/xargs.1.html), которая выполнит команду, используя STDIN в качестве аргументов.
Например `ls | xargs rm` удалит файлы в текущем каталоге.

	Ваша задача — написать команду, которая рекурсивно находит все HTML-файлы в папке и архивирует их. Обратите внимание, что ваша команда должна работать, даже если в файлах есть пробелы (подсказка: проверьте флаг `-d` для `xargs`).
Для создания файлов можете использовать данный скрипт:
	```bash
	for i in {1..100}; do 
		file_extension=('.HTML'  '.txt'  '.cpp'  '.o'  '.so') 
		file_type=${file_extension[$RANDOM % ${#file_extension[@]}]} 
		file_name=generated_file_$i$file_type  
		touch  $file_name  
		echo  "This is file number $i" > $file_name  
	done
	```  

> Если вы используете macOS, обратите внимание, что функция поиска BSD по умолчанию отличается от той, которая включена в [GNU coreutils](https://en.wikipedia.org/wiki/List_of_GNU_Core_Utilities_commands). Вы можете использовать `-print0` для `find` и флаг `-0` для `xargs`. Как пользователь macOS, вы должны знать, что утилиты командной строки, поставляемые с macOS, могут отличаться от аналогов GNU; вы можете установить версии GNU, если хотите, [используя Brew](https://formulae.brew.sh/formula/coreutils).
--------
5. Напишите команду или сценарий для рекурсивного поиска последнего измененного файла в каталоге. В более общем плане, можете ли вы перечислить все файлы по давности?
