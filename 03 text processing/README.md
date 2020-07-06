-- 03-a-0200  
Сортирайте /etc/passwd лексикографски по поле UserID.
```
sort -t: -k3,3 /etc/passwd
```
-- 03-a-0201  
Сортирайте /etc/passwd числово по поле UserID.
(Открийте разликите с лексикографската сортировка)
```
sort -t: -k3 -n /etc/passwd
```
-- 03-a-0210  
Изведете само 1-ва и 5-та колона на файла /etc/passwd спрямо разделител ":".
```
cut -d ":" -f 1,5 /etc/passwd
```
-- 03-a-0211  
Изведете съдържанието на файла /etc/passwd от 2-ри до 6-ти символ.
```
cut -c 2-6 /etc/passwd
```
-- 03-a-1500  
Намерете броя на символите в /etc/passwd. А колко реда има в /etc/passwd?  
символи -> ```wc -c /etc/passwd```  
редове -> ```wc -l /etc/passwd```  

-- 03-a-2000  
Извадете от файл /etc/passwd:  
- първите 12 реда  
```
head -12 /etc/passwd
```
- първите 26 символа 
```
cut -c -26 /etc/passwd
```

- всички редове, освен последните 4  
```
head -n -4 /etc/passwd
```

- последните 17 реда  
```
tail -n 17 /etc/passwd
```

- 151-я ред (или друг произволен, ако нямате достатъчно редове)  
```
sed '151!d' /etc/passwd
```

- последните 4 символа от 13-ти ред  
```
sed '13!d' /etc/passwd | grep -o '....$'
```
-- 03-a-2100  
Отпечатайте потребителските имена и техните home директории от /etc/passwd.
```
cut -d ":" -f 1,6 /etc/passwd
```
-- 03-a-2110  
Отпечатайте втората колона на /etc/passwd, разделена сcutпрямо символ '/'.
```
cut -d "/" -f 2 /etc/passwd
```

-- 03-a-3000  
Запаметете във файл в своята home директория резултатът от командата ls -l изпълнена за вашата home директорията.
Сортирайте създадения файла по второ поле (numeric, alphabetically).
```
ls -l /home/silwiers >> /home/silwiers/myHome.txt
sort -k2 /home/silwiers/myHome.txt
```
```
sort -nk7 /home/silwiers/myHome.txt 
```
-- 03-a-5000  
Отпечатайте 2 реда над вашия ред в /etc/passwd и 3 реда под него // може да стане и без пайпове
```
grep -B 2 -A 3 '^silwiers' /etc/passwd
```
-- 03-a-5001  
Колко хора не се казват Ivan според /etc/passwd  
```
grep -i -v 'Ivan' /etc/passwd | wc -l
```
-- 03-a-5002  
Изведете имената на хората с второ име по-дълго от 7 (>7) символа според /etc/passwd
```
cat /etc/passwd | grep "\/bin/bash" | cut -d ":" -f 5 | cut -d "," -f 1 | grep -E "[a-z]{8}$"
```
-- 03-a-5003  
Изведете имената на хората с второ име по-късо от 8 (<=7) символа според /etc/passwd // !(>7) = ?
```
grep "\/bin/bash" /etc/passwd | cut -d ":" -f 5 | cut -d "," -f 1 | grep -v -E "[a-z]{7}$"
```

-- 03-a-5004  
Изведете целите редове от /etc/passwd за хората от 03-a-5003
```
grep -E '^([a-zA-Z]|[0-9])+:x:[0-9]+:[0-9]+:[a-zA-Z]+ [a-zA-Z]{1,7}\b' /etc/passwd
```

-- 03-b-0300  
Намерете факултетния си номер във файлa /etc/passwd.
```
cat /etc/passwd | grep '61909' | cut -d ":" -f1 | cut -d "s" -f2
```

-- 03-b-3000  
Запазете само потребителските имена от /etc/passwd във файл users във вашата home директория.
```
cut -d ":" -f1 /etc/passwd >> users.txt
```
-- 03-b-3400  
Колко коментара има във файла /etc/services ? Коментарите се маркират със символа #, след който всеки символ на реда се счита за коментар.
```
cut -d "#" -f 2 /etc/services | wc -m
```

-- 03-b-3450  
Вижте man 5 services. Напишете команда, която ви дава името на протокол с порт естествено число N. Командата да не отпечатва нищо, ако търсения порт не съществува (например при порт 1337). Примерно, ако номера на порта N е 69, командата трябва да отпечати tftp
```
cat /etc/services | grep 1433/ | head -n 1 | awk '{print $1;}'
```

-- 03-b-3500  
Колко файлове в /bin са shell script? (Колко файлове в дадена директория са ASCII text?)
```
find /bin -mindepth 1 -type f -exec grep -o '#!/bin/sh' {} \; | wc -l
```

-- 03-b-3600  
Направете списък с директориите на вашата файлова система, до които нямате достъп. Понеже файловата система може да е много голяма, търсете до 3 нива на дълбочина.
```
find . -maxdepth 3 -type d 2>&1 1>/dev/null | cut -d ":" -f 2 | sed -e 's/^..\(.*\).$/\1/g'
```
А до кои директории имате достъп?  
```
find . -maxdepth 3 -type d | grep -v 'Permission denied'
```

Колко на брой са директориите, до които нямате достъп?  
```
find . -maxdepth 3 -type d 2>&1 1>/dev/null | cut -d ":" -f 2 | sed -e 's/^..\(.*\).$/\1/g' | wc -l
```
-- 03-b-4000  
Създайте следната файлова йерархия.  
/home/s...../dir1/file1  
/home/s...../dir1/file2  
/home/s...../dir1/file3  
```
mkdir dir1  
cd dir1  
touch file1 file2 file3  
```

Посредством vi въведете следното съдържание:  
file1:  
1  
2  
3  
  
file2:  
s  
a  
d  
f  
  
file3:  
3  
2  
1  
45  
42  
  
1  
52  
  
Изведете на екрана:  
* статистика за броя редове, думи и символи за всеки един файл  
```
printf "Lines:\n$(wc -l file*)\nWords:\n$(wc -w file*)\nCHars:\n$(wc -c file*)\n" | grep -v 'total'
```

* статистика за броя редове и символи за всички файлове  
```
printf "Lines:\n$(wc -l file*)\nCHars:\n$(wc -c file*)\n" | grep -v 'file'
```

* общия брой редове на трите файла  
```
printf "Lines:\n$(wc -l file*)\nWords:\n$(wc -w file*)\nCHars:\n$(wc -c file*)\n" | grep -v 'file'
```

-- 03-b-4001  
Във file2 подменете всички малки букви с главни.
```
tr [a-z] [A-Z] < dir1/file2
```

-- 03-b-4002  
Във file3 изтрийте всички "1"-ци.
```
sed 's/1//g' dir1/file3
```
-- 03-b-4003  
Изведете статистика за най-често срещаните символи в трите файла.  
3те общо ->  ```cat * | sort | uniq -c ``` - всички символи  
само най-повтарящите се - >  ```cat dir1/file* | sort | uniq -d```  
най-повтaрящите се + колко пъти ->```cat dir1/file* | sort | uniq -dc```  
най-горния само -> ```cat dir1/file* | sort | uniq -dc | head -1```

-- 03-b-4004  
Направете нов файл с име по ваш избор, който е конкатенация от file{1,2,3}. 
Забележка: съществува решение с едно извикване на определена програма - опитайте да решите задачата чрез него.
```
cat dir1/file* >> concFiles.txt
```

-- 03-b-4005 
Прочетете текстов файл file1 и направете всички главни букви малки като запишете резултата във file2.
```
cat file1 | tr [A-Z] [a-z] >> file2
``` 
-- 03-b-5200  
Изтрийте всички срещания на буквата 'a' (lower case) в /etc/passwd и намерете броят на оставащите символи.
```
sed 's/a//g' /etc/passwd > passwd-cpy
wc -c passwd-cpy
```
  
  ```
cat /etc/passwd | sed 's/a//g' | wc -m
```
  
-- 03-b-5300  
Намерете броя на уникалните символи, използвани в имената на потребителите от /etc/passwd.
```
cut -d ":" -f 1 /etc/passwd | sed 's/\(.\)/\1\n/g' | sort | uniq -c | wc -l
```

-- 03-b-5400  
Отпечатайте всички редове на файла /etc/passwd, които не съдържат символния низ 'ov'.
```
grep -v 'ov' /etc/passwd
```

-- 03-b-6100  
Отпечатайте последната цифра на UID на всички редове между 28-ми и 46-ред в /etc/passwd.
```
sed -n 28,46p /etc/passwd | cut -d ":" -f 3 | grep -o '.$'
```

-- 03-b-6700  
Отпечатайте правата (permissions) и имената на всички файлове, до които имате read достъп, намиращи се в директорията /tmp.
```
ls -l /tmp | grep -E '^-r' | awk '{print $1, $NF}'
```

-- 03-b-6900  
Намерете имената на 10-те файла във вашата home директория, чието съдържание е редактирано най-скоро. На първо място трябва да бъде най-скоро редактираният файл.
```
ls -lt | grep -v 'total' | awk '{print $NF}' | head -10
```

-- 03-b-7000  
Файловете, които съдържат C код, завършват на `.c`.  
Колко на брой са те във файловата система (или в избрана директория)? 
```
find -maxdepth 5 -name '*.c' 2>/dev/null | wc -l
```

Колко реда C код има в тези файлове?  
```
find . -maxdepth 5 -name '*.c' 2>/dev/null | xargs wc -l
```
-- 03-b-7500  
Даден ви е ASCII текстов файл (например /etc/services). Отпечатайте хистограма на N-те (например 10) най-често срещани думи.  
```
cat /etc/services | grep -Eo '\S+'  
| grep -Eo '[a-zA-Z]+'   
| sort | uniq -c | sort -nr  
| head -10  
| awk '{t = $1; $1 = $2; $2 = t; print;}'   
| awk '{$2=sprintf("%-*s", $2, ""); gsub(" ", "=", $2); printf("%-10s%s\n", $1, $2)}'   
```
-- 03-b-8000  
Вземете факултетните номера на студентите от СИ и ги запишете във файл si.txt сортирани.  
```
cat /etc/passwd | grep -E '^s' | grep -E 'SI'| cut -d ":" -f 1 | cut -d "s" -f 2 | sort -n >> si.txt
```

-- 03-b-8500  
За всеки логнат потребител изпишете "Hello, потребител", като ако това е вашият потребител, напишете "Hello, потребител - this is me!".  
```
cat /etc/passwd | cut -d ":" -f 1
| awk '{printf "Hello, "; print}'
| sed '/Hello, silwiers/s/$/ - this is me!/'
```
Пример:  

hello, human - this is me!  
Hello, s63465  
Hello, s64898  
  
-- 03-b-8520  
Изпишете имената на студентите от /etc/passwd с главни букви.
```
cat /etc/passwd | cut -d ":" -f 1 | tr [a-z] [A-Z]
```

-- 03-b-8600  
Shell Script-овете са файлове, които по конвенция имат разширение .sh. Всеки такъв файл започва с "#!<interpreter>" , където <interpreter> указва на операционната система какъв интерпретатор да пусне (пр: "#!/bin/bash", "#!/usr/bin/python3 -u").  

Намерете всички .sh файлове и проверете кой е най-често използваният интерпретатор.  
```
find -maxdepth 4 -name '*.sh' 2>/dev/null | xargs head -1 | grep -E '^#!/' | sort | uniq -c | head -1
```

-- 03-b-8700  
Намерете 5-те най-големи групи подредени по броя на потребителите в тях.  
```
awk -F : - : is the separator // awk -F '[,:]'  
getent group | awk -F '[,:]' '{print $1, NF -3}' | sort -k2,2nr | head -5
```
-- 03-b-9000  
Направете файл eternity. Намерете всички файлове, които са били модифицирани в последните 15мин (по възможност изключете .). Запишете във eternity името на файла и часa на последната промяна.  
```
touch eternity
```
```
find -type f -mmin -15 -ls  
| awk '{print $10, $11}'  
| awk -F / '{print $1,$NF}'  
| awk '{t = $1; $1 = $2; $2 = t; print;}'  
| column -t > eternity   
```
-- 03-b-9050  
Копирайте файл /home/tony/population.csv във вашата home директория.
```
cp /home/tony/population.csv population1.csv  
```

-- 03-b-9051  
Използвайки файл population.csv, намерете колко е общото население на света през 2008 година. А през 2016?
```
cat population.csv | grep -E ',2016,' | cut -d "," -f4 | paste -sd+ - | bc
```

-- 03-b-9052  
Използвайки файл population.csv, намерете през коя година в България има най-много население.
```
cat population.csv | grep -E 'Bulgaria'| tr , ' ' | sort -n -k4 | tail -1 | cut -d " " -f 3
```

-- 03-b-9053  
Използвайки файл population.csv, намерете коя държава има най-много население през 2016. А коя е с най-малко население?  
(Hint: Погледнете имената на държавите)  
*имената съдържат интервали, точки, тирета и т.н.  
най-много ->  
```cat population.csv  
| grep -E ',2016,'  
| awk -F , '{print $NF,$0}'  
| sort -n -k1 | tail -1  
| cut -f2- -d ' '  
| awk -F , 'NF{NF-=3};1'  
```
най-малко ->  
```
cat population.csv | grep -E ',2016,' | awk -F , '{print $NF,$0}' | sort -n -k1 | head -1 | cut -f2- -d ' '| awk -F , 'NF{NF-=3};1'
```
-- 03-b-9054  
Използвайки файл population.csv, намерете коя държава е на 42-ро място по население през 1969. Колко е населението й през тази година?  
```
grep "$(cat population.csv | sort | uniq   
| grep ',1969,'   
| sed -r "s/^(.*),.+,([0-9]+)$/\1:\2/"  
| sort -nr -t ':' -k2  
| head -42 | tail -1  
| cut -d , -f1)" population.csv  
| uniq | grep ',2016,' | grep -Eo '[0-9]+$'  
```

-- 03-b-9100  
В home директорията си изпълнете командата `curl -o songs.tar.gz "http://fangorn.uni-sofia.bg/misc/songs.tar.gz"`  
  
-- 03-b-9101  
Да се разархивира архивът songs.tar.gz в папка songs във вашата home директорията.  
```
tar -xzvf songs.tar.gz -C ~/songs
```
-- 03-b-9102  
Да се изведат само имената на песните.  
```
ls /home/silwiers/songs | cut -d '(' -f1 | cut -d '-' -f2 | sed 's/^.//'
```
-- 03-b-9103  
Имената на песните да се направят с малки букви, да се заменят спейсовете с долни черти и да се сортират.
```
ls /home/silwiers/songs  
| cut -d '(' -f1  
| cut -d '-' -f2  
| sed 's/^.//'  
| tr [A-Z] [a-z] | tr ' ' '_'  
| sed 's/_$//' | sort  
```

-- 03-b-9104  
Да се изведат всички албуми, сортирани по година.  
```
ls /home/silwiers/songs  
| cut -d '(' -f2 | cut -d ')' -f1  
| awk '{print $NF,$0}'  
| sort -nr | cut -f2- -d ' ' | uniq  
```
-- 03-b-9105  
Да се преброят/изведат само песните на Beatles и Pink.
извеждане:  
```
ls /home/silwiers/songs | grep -E '^(Beatles|Pink -)'
```
преброяване:
```
ls /home/silwiers/songs | grep -E '^(Beatles|Pink -)'| wc -l
```
-- 03-b-9106  
Да се направят директории с имената на уникалните групи. За улеснение, имената от две думи да се напишат слято:  
Beatles, PinkFloyd, Madness  
```
ls /home/silwiers/songs  | cut -d '-' -f1 | sed 's/ //' | uniq | xargs mkdir
```
-- 03-b-9200  
Напишете серия от команди, които извеждат детайли за файловете и директориите в текущата директория, които имат същите права за достъп както най-големият файл в /etc директорията.  
```
find /etc -printf "%s %p %m\n" 2>/dev/null | sort -n | tail -1 | cut -d ' ' -f3 | xargs find . -maxdepth 1 -perm
```
