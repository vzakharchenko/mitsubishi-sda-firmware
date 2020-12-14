


===================

# Проект по структуре прошивки для Kenwood MXLO15KLG4.

На основе структура прошивки  1G(S_V4_2_0206_0500) для Mitsubishi Outlander.  

Структура прошивки анологична для других автомобилей как Mitsubishi ASX, Pajero, Lancer  

| Папка                        |  Описание                                                           |
|------------------------------|---------------------------------------------------------------------|
| NK_V4_2_0206_0500_8G         | [Андроид 4.2.2 c загрузчиком](#%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80%D0%B0-%D1%84%D0%B0%D0%B9%D0%BB%D0%B0-navink_v4_2_0206_0500nfu)                                         |
| UpdateVersionControl.ini     | [Описание файла прошивки](#UpdateVersionControlini)                                             |
| BT_V21C                      | Прошивка для Bluetooth модуля                                       |
| PANEL_V4_2_0106_0500         | ?????? ??                                                           |
| UBOOT_V4_2_0206_0500_release | Uboot                                                               |
| MAIN_V4_2_0206_0500          | ?????????                                                           |
| DB_V0.0.006.1300             | ?????????                                                           |
| TCONF_V1_9_0006_0500         | ?????????                                                           |
| TOUCH_V0_0_2246_0200         | ?????????                                                           |
| SXM_V0B2A01                  | Прошивка для SXM модуля                                             |


# UpdateVersionControl.ini
```
#
#S_V4_2_0206_0500
#
BT_V21C
DB_V0.0.006.1300
MAIN_V4_2_0206_0500
NK_V4_2_0206_0500_8G
PANEL_V4_2_0106_0500
SXM_V0B2A01
TCONF_V1_9_0006_0500
TOUCH_V0_0_2246_0200
UBOOT_V4_2_0206_0500_release
# 
# end
#
```


# Структура файла NaviNk_V4_2_0206_0500.nfu

| Начало  | Конец   | Описание                                                            |
|---------|---------|---------------------------------------------------------------------|
| 20      | 22      | Хеш сумма zip файла с system.img, Алгоритм CRC16-CCITT Kermit       |
| 90      | 9f      | Идетификатор прошивки                                               |
| 200     | 40200   | bios.bin                                                            |
| 40200   | 80500   | s-bios.bin                                                          |
| 80500   | 1480000 | boot.img прошивки можно разобрать и собрать unmkbootimg и mkbootimg |
| 1480000 | EOF     | zip файл с system.img и с бутлоадером                               |

# Структура Zip файла из NaviNk_V4_2_0206_0500.nfu

| Файл             | Описание                                                            |
|------------------|---------------------------------------------------------------------|
| datainfo         | Файл с Хеш файлов из bin, Алгоритм CRC16-CCITT Kermit               |
| bin/qboot.bin    | qboot.bin                                                           |
| bin/bios.bin     | bios.bin                                                            |
| bin/s-bios.bin   | s-bios.bin                                                          |
| bin/partition    | partition                                                           |
| bin/system.img   | образ Android 4.2.2 со всеми приложениями                           |
| bin/boot.img     | boot.img прошивки можно разобрать и собрать unmkbootimg и mkbootimg |

# Структура datainfo из Zip файла прошивки NaviNk_V4_2_0206_0500.nfu
```
boot.img,1,e266,808000
qboot.bin,1,dee5,5377000
update_uImage,1,d42,8fe000
RTFM_SH4A_MA100.bin,0,0,0
system.img,1,ba5b,15c00000
bios.bin,1,5105,c96c
s-bios.bin,1,a016,4a24
```
```
<имя файла из zip>,1,<Хеш сумма по алгоритму CRC16-CCITT Kermit>, <Размер файла>
```

# редактирование NaviNk_V4_2_0206_0500.nfu
1. редактируем
2. вычисляем  checksum 8 bit 
3. обновляем файл SumList.ini который рядом с NaviNk_V4_2_0206_0500.nfu
```
NaviNk_V4_2_0306_0500.nfu : <8 bit checksum>
```

# редактирование zip файла из NaviNk_V4_2_0206_0500.nfu
1. извлекаем файл например WinHex
2. редактируем
3. вычисляем  Хеш (CRC16-CCITT Kermit)
4. обновляем файл прошивки
5. Не забываем обновить Хеш  по адресу 0x20

# редактирование system.img
1. Монтируем раздел в директорию system 
```
sudo mkdir system
sudo mount -rw system.img system
```
2. Заходим в папку  system и редактируем раздел.
3. Размонтируем раздел
4. файл system.img  изменился и нужно обновить Хеш в файле datainfo (CRC16-CCITT Kermit)

