# 03-sysadmin-02-terminal  

 ## 1. Какого типа команда `cd?` Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.

Это встроенная команда оболочки, какой и должна быть. Назначение у неё одно и вполне конкретное, что вполне в духе linux: одна команда - одно действие. 
```
vagrant@vagrant:~$ type cd
cd is a shell builtin
```

---

 ## 2. Какая альтернатива без pipe команде `grep <some_string> <some_file> | wc -l`? `man grep` поможет в ответе на этот вопрос. Ознакомьтесь с [документом](http://www.smallo.ruhr.de/award.html) о других подобных некорректных вариантах использования pipe.  

Здесь вполне сойдёт `grep <some string> <some file> -c` если нужно просто узнать количество строк с совпадениями. 

---

 ## 3. Какой процесс с PID `1` является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?  

Команда pstree -p  говорит что это systemd  
```
vagrant@vagrant:~$ pstree -p
systemd(1)─┬─VBoxService(798)─┬─{VBoxService}(800)
           │                  ├─{VBoxService}(801)
           │                  ├─{VBoxService}(802)
           │                  ├─{VBoxService}(803)
           │                  ├─{VBoxService}(805)
           │                  ├─{VBoxService}(806)
           │                  ├─{VBoxService}(807)
           │                  └─{VBoxService}(808)
           ├─accounts-daemon(577)─┬─{accounts-daemon}(609)
           │                      └─{accounts-daemon}(642)
```

---

 ## 4. Как будет выглядеть команда, которая перенаправит вывод stderr `ls` на другую сессию терминала?  

Смотрим номер своего терминала:
```
vagrant@vagrant:~$ tty
/dev/pts/0
```

Смотрим список открытых терминалов:
```
vagrant@vagrant:~$ lsof | grep pts
systemd   2196                       vagrant  mem       REG              253,0   454192     527401 /usr/lib/x86_64-linux-gnu/libcryptsetup.so.12.5.0
bash      2234                       vagrant    0u      CHR              136,0      0t0          3 /dev/pts/0
bash      2234                       vagrant    1u      CHR              136,0      0t0          3 /dev/pts/0
bash      2234                       vagrant    2u      CHR              136,0      0t0          3 /dev/pts/0
bash      2234                       vagrant  255u      CHR              136,0      0t0          3 /dev/pts/0
bash      2475                       vagrant    0u      CHR              136,1      0t0          4 /dev/pts/1
bash      2475                       vagrant    1u      CHR              136,1      0t0          4 /dev/pts/1
bash      2475                       vagrant    2u      CHR              136,1      0t0          4 /dev/pts/1
bash      2475                       vagrant  255u      CHR              136,1      0t0          4 /dev/pts/1
lsof      2497                       vagrant    0u      CHR              136,0      0t0          3 /dev/pts/0
lsof      2497                       vagrant    2u      CHR              136,0      0t0          3 /dev/pts/0
grep      2498                       vagrant    1u      CHR              136,0      0t0          3 /dev/pts/0
grep      2498                       vagrant    2u      CHR              136,0      0t0          3 /dev/pts/0
```

Выводим ошибку в другой терминал:
```
vagrant@vagrant:~$ ls <несуществ директория> 2>/dev/pts/1
```

---

 ## 5. Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.  
 
 К примеру вот: 
 ```
 vagrant@vagrant:~$ ls /dev > list | grep -n 'loop*' list  | tee loop_list
28:loop0
29:loop1
30:loop2
31:loop3
32:loop4
33:loop5
34:loop6
35:loop7
36:loop-control
```
т.е. выводим список файловв и директорий в файл "list", затем сортируем его и выводим результат сортировки в другой файл.  

---

 ## 6. Получится ли находясь в графическом режиме, вывести данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?  
 


 
 
