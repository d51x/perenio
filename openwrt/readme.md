### Прошиваем OpenWRT на Perenio

Инструкция от [DivanX10](https://github.com/DivanX10/Openwrt-scripts-for-gateway-zhwg11lm/wiki/%D0%A1%D1%82%D0%B0%D0%B2%D0%B8%D0%BC-OpenWRT-%D0%BD%D0%B0-%D1%88%D0%BB%D1%8E%D0%B7-Perenio-PEACG01 "DivanX10") 

## Альтернативная инструкция здесь:

### Преимущество:
1. Не надо претыкать кабель из компа в роутер и обратно
2. Не надо лезть в настройки сетевой карты на компе

#### Процесс установки:
1. Загружаем на компе `tftp` server (в моем случае он имеет IP 192.168.2.47 - адрес компа в сети)
2. Подключаем Perenio кабелем к роутеру
3. Фиксируем на роутере постоянный IP адрес (у меня 192.168.2.6)
4. Загружаемся в стоковый OpenWRT на Perenio и меняем параметр bootdelay на 5
```
setenv bootdelay 5
saveenv
```
5. Перезагружаем Perenio и входим в `boot` (>ash)
6. Выполняем команды ниже (изменяем ip адрес, куда будет идти Preneio по tftp, нужно, чтобы не подключать Perenio к компу и не менять настройки сетевой карты)
```
   setenv ipaddr 192.168.2.6           (адрес Perenio на роутере)
   setenv serverip 192.168.2.47       (адрес компа в сети)
```
7.  Выполняем команду `tftpboot`, по умолчанию адрес у Perenio `192.168.1.2` и поэтому `tftp` ищет [файл](https://github.com/d51x/perenio/blob/main/openwrt/0201A8C0.img "файл") `0201A8C0.img` (0201 - это из IP адреса 1.2, т.е. это перевернутый IP адрес в  HEX, C0 = 192, A8 = 168, 01 = 1, 02 = 2). Видим лог
```
	ath> tftpboot
	Trying eth0
	dup 1 speed 100
	*** Warning: no boot file name; using '0602A8C0.img'
	Using eth0 device
	TFTP from server 192.168.2.47; our IP address is 192.168.2.6
	Filename '0602A8C0.img'.
```
В моем случае tftp ищет файл `0602A8C0.img` (0602 - это из ip адреса 2.6).
Просто  переименуем или сделаем копию файла `0201A8C0.img` -> `0602A8C0.img` (в вашем случае подставляем то имя, которое ищет tftp)

8. Запускаем `tftpboot` еще раз, ожидаем загрузки файла
9. После успешной загрузки выполняем команду `bootm`
10. Переходим в браузере по адресу из п.3 (192.168.2.6) и ждем загрузки OpenWRT
11. Далее через Восстановление (sysupgrade) прошиваем актуальную прошивку Perenio OpenWRT
