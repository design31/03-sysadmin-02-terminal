# 03-sysadmin-02-terminal  

 ## 1. Какого типа команда `cd?` Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.

Это встроенная команда оболочки, какой и должна быть. Назначение у неё одно и вполне конкретное, что вполне в духе linux: одна команда - одно действие. 

---

 ## 2. Какая альтернатива без pipe команде `grep <some_string> <some_file> | wc -l`? `man grep` поможет в ответе на этот вопрос. Ознакомьтесь с [документом](http://www.smallo.ruhr.de/award.html) о других подобных некорректных вариантах использования pipe.  

Здесь вполне сойдёт `grep <some string> <some file> -c` если нужно просто узнать количество строк с совпадениями. 

---

3. Какой процесс с PID `1` является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?  

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

4. Как будет выглядеть команда, которая перенаправит вывод stderr `ls` на другую сессию терминала?  

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

Выводим в другой:
```
vagrant@vagrant:~$ ls > /dev/pts/1
```

---

