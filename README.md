## Задание

Необходимо настроить виртуальную машину А с Ubuntu (желательно, но можно и другую Linux подобную ОС) в VirtualBox.
Обеспечить доступ в сеть Интернет. Осуществить проверку этого доступа и приложить скриншот из терминала.
Следующим шагом настроить ещё одну виртуальную машину Б.
После чего обеспечить сетевой доступ от машины А к машину Б. Приложить скриншот из терминала.
Поднять ещё одну виртуальную машину В. Организовать сетевой доступ:

1. из машины А в машину Б,
2. из машины А в машину В,
3. но запретить доступ из машины Б в машину В,
4. приложить скриншот, на котором видно терминалы всех трёх машин и видно что между машинами есть (или нет) доступа.

## Решение

Для этого в настройках каждой из машин в VirtualBox зайдём в раздел "Сеть" и выберем тип подключения "Внутренняя сеть" (Internal network). Для того, чтобы они все оказались в одной сети, называем их одинаково (чего я сначала не понял). Заходим в файл `/etc/netplan/01-netcfg.yaml` и вписываем эти настройки:

![image](https://github.com/mxrget/linux-university-lab-3/blob/main/pic1.png)
*Для второй ВМ будет использоваться адрес 192.168.1.20, для третьей - 192.168.1.30*

Вводим команду `ip a` - проверяем, назначился ли IP-адрес: да, под интерфейсом enp0s3 есть строка с `inet 192.168.1.10/24`. 
![image](https://github.com/mxrget/linux-university-lab-3/blob/main/pic2.png)

Теперь 3 машины могут пинговать друг друга.
![image](https://github.com/mxrget/linux-university-lab-3/blob/main/pic3.png)

*Вторая машина успешно пингует первую.*

![image](https://github.com/mxrget/linux-university-lab-3/blob/main/pic4.png)

*Третья машина успешно пингует вторую.*

Осталось разобраться, как запретить доступ от второй машины к третьей. Изначально на третьей я поставил фаерволл (`sudo ufw deny from 192.168.1.20`), но это почему-то не сработало, поэтому я ввёл `sudo iptables -I INPUT -s 192.168.1.20 -j DROP`, и теперь оно действительно запретило доступ от второй машины к третьей.

![image](https://github.com/mxrget/linux-university-lab-3/blob/main/pic5.png)

*К одной машине пинг проходит, к другой - нет.*

Ну и красивая (не очень, потому что я подключился к монитору и всё на ВМах зашакалилось) картиночка, где видны все три ВМ, некоторые из которых могут друг друга пинговать, а некоторые - нет.

![image](https://github.com/mxrget/linux-university-lab-3/blob/main/pic6.png)

*Задание выполнено.*
