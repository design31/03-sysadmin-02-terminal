# 03-sysadmin-02-terminal  

1. Какого типа команда `cd?` Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.

Это встроенная команда оболочки, какой и должна быть. Назначение у неё одно и вполне конкретное. 

3. Какой процесс с PID `1` является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?  

vagrant@vagrant:~$ pstree -p  говорит что это systemd  
```vagrant@vagrant:~$ pstree -p
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
