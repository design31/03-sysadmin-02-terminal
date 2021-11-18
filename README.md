# 03-sysadmin-02-terminal  

 ### 1. Какого типа команда `cd?` Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.

Это встроенная команда оболочки, какой и должна быть. Назначение у неё одно и вполне конкретное, что вполне в духе linux: одна команда - одно действие. 
```
vagrant@vagrant:~$ type cd
cd is a shell builtin
```

---

 ### 2. Какая альтернатива без pipe команде `grep <some_string> <some_file> | wc -l`? `man grep` поможет в ответе на этот вопрос. Ознакомьтесь с [документом](http://www.smallo.ruhr.de/award.html) о других подобных некорректных вариантах использования pipe.  

Здесь вполне сойдёт `grep <some string> <some file> -c` если нужно просто узнать количество строк с совпадениями. 

---

 ### 3. Какой процесс с PID `1` является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?  

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

 ### 4. Как будет выглядеть команда, которая перенаправит вывод stderr `ls` на другую сессию терминала?  

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

 ### 5. Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.  
 
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

 ### 6. Получится ли находясь в графическом режиме, вывести данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?  
 
Не понял вопрос. Надо установить виртуалку с графическим окружением и что-то из него сделать? Все терминалы у меня tty. Что локальные, что ssh. Что значит “вывести данные из PTY в какой-либо из эмуляторов TTY”?  
Между терминалами всё отлично перенаправляется/выводится.  
 ```
 us@ubuntu:~$ tty
/dev/pts/1
us@ubuntu:~$ echo "Proverka svyazi" > /dev/pts/0
```

---


 ### 11. Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью `/proc/cpuinfo`.  
 
Версию 4_2 судя по всему:
```
vagrant@vagrant:~$ grep sse /proc/cpuinfo
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq ssse3 cx16
pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx rdrand hypervisor lahf_lm abm invpcid_single pti fsgsbase avx2 invpcid md_clear flush_l1d
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq ssse3 cx16
pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx rdrand hypervisor lahf_lm abm invpcid_single pti fsgsbase avx2 invpcid md_clear flush_l1d
```

---

 ### 12. При открытии нового окна терминала и vagrant ssh создается новая сессия и выделяется pty. Это можно подтвердить командой tty, которая упоминалась в лекции 3.2. Однако:
```
vagrant@netology1:~$ ssh localhost 'tty'
not a tty
```
 ### Почитайте, почему так происходит, и как изменить поведение. 

При отправке команд по ssh псевдотерминал по умолчанию не создаётся, поэтомы мы получаем такую ошибку. Принудительно создать tty можно ключом `-t`,  втаком случае команда создает tty и показывает его номер:
```
vagrant@vagrant:~$ ssh -t localhost tty
vagrant@localhost's password:
/dev/pts/3
Connection to localhost closed.
```

---

 ### 13. Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись `reptyr`. Например, так можно перенести в `screen` процесс, который вы запустили по ошибке в обычной SSH-сессии.  
 
Получилось :)

---

 ### 14. `sudo echo string > /root/new_file` не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без `sudo` под вашим пользователем. Для решения данной проблемы можно использовать конструкцию `echo string | sudo tee /root/new_file`. Узнайте что делает команда `tee` и почему в отличие от `sudo echo` команда с `sudo tee` будет работать.  
 
В случае с `sudo tee` которая делает вывод одновременно на стандартное устройство вывода+в файл перенаправления не требуется и команда сразу пишет в файл от имени суперпользователя. 

 
