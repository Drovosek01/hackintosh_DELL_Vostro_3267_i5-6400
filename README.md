# Что я делал при установки Hackintosh на системный блок Dell Vostro 3267

[ENGLISH](/docs/ENG/README.md) | РУССКИЙ

Эту инструкцию я писал для себя, чтобы после переустановки не вспоминать что и как делать. Инструкция написана после установки macOS Mojave 10.14.6. на системный блок [DELL Vostro 3267](https://www.dell.com/support/manuals/vostro-3268-desktop/3268_om_pub/).

## Характеристики системного блока Dell Vostro 3267

* Процессор
  * Intel Core i5-6400 2.7GHz, Turboboost 3.6GHz, Skylake
* Видеокарта
  * Intel HD Graphics 530
* ОЗУ
  * 8 GB DDR4 2400MHz
* Системная плата
  * Dell Vostro 3267
* Звуковой адаптер
  * Realtek ALC 891
* Bluetooth
  * Dell Wireless 1707 Bluetooth 4.0 LE Device  
* Wifi
  * Qualcomm QCA9565 802.11b/g/n Wireless Adapter
* Ethernet
  * Realtek RTL8168/8111 PCI-E Gigabit Ethernet Adapter

## Что получилось и не получилось "оживить"

### Что работает

* Графика Intel HD Graphics 530
* Графическое ускорение Intel Quick Sync
* TurboBoost
* SpeedStep
* USB 3.0 и USB 2.0 порты
* Порт 3.5 mini Jack
* Ethernet

### Что не работает

* D-Sub (VGA)
* FaceTime, iMassage, HandOff, Continuity - для того, чтобы заставить работать эти функции нужно, чтобы работал Wifi (и, наверное, bluetooth, но это не точно). Возможно нужно купить совместимые с macOS Mojave Wifi модули и заменить или подключить их, а может можно заставить работать существующие Wifi и bluetooth модули, но я не захотел этим заниматься, потому что использую интернет по Ethernet проводу

### Что не проверено

* Wifi
* Bluetooth
* HDMI Audio
* Дисковод (он распознается в macOS и его можно даже открыть нажав пару кнопок в интерфейсе, но у меня нет DVD/CD дисков, чтобы проверить работу дисковода)

## Предустановочная подготовка

На всякий случай сохраните все важные файла на отдельную флешку или в облако.

Сделайте отчет системы в AIDA64 и заархивируйте папку EFI, которая у вас на флешке, и загрузите в облако, чтобы в случае непредвиденной ситуации вы могли грамотно описать ситуацию прикрепив файлы с описанием и, чтобы вам смогли помочь.

MacOSX может быть установлена на диск (без форматирования) только с разметкой GPT (GUID Partition Table).

Рекомендуется во время установки MacOSX стереть (отформатировать) весь диск, но это не обязательно.

Если у вас только 1 диск (1 HDD или 1 SSD), на котором уже установлена Windows и вы хотите установить MacOSX рядом с Windows, то есть несколько вариантов решения:

* Во время установки MacOSX стереть (отформатировать) весь диск, но тогда будут удалены все файлы
* Купить отдельный диск и установить MacOSX на него
* Если ваш диск размечен в GPT, то можно в Windows отделить раздел и потом на него установить MacOSX
  * Узнать как размечен можно в Windows, для этого откройте "Управление дисками" (нажмите ПКМ по пуску), потом откройте свойства диска (нажмите ПКМ по самому диску, а не по разделам, там где написано "Диск 0" или "Диск 1"), откройте вкладку "Тома" и там посмотрите стиль раздела. Если стиль раздела "Таблица с GUID разделов", то все хорошо
  * Отделить или создать раздел в WIndows можно в той же программе "Управление дисками", для этого нажимаете ПКМ по разделу, от которого будете отделять другой раздел и в конектном меню нажимаете "Сжать" и дальше выбираете до скольки мегабайт сжать выбранный раздел. Соответственно все, что больше этих мегабайт сформируется в новую неразмеченную облать, которую вы можете разметить в любую файловую систему, но лучше в FAT32. Потом во время установки MacOSX нужно в Дисковой утилите выбрать этот раздел, отформатировать в APFA или HFS+ (зависит от версии MacOSX, которую вы устанавливаете) и при установке MacOSX выбирать этот раздел
* Если ваш диск размечен в MBR, то вам нужно будет его конвертировать в GPT, либо стереть (отформатировать) в Дисковой утилите во время установки MacOSX
  * Узнать как размечен можно в Windows, для этого откройте "Управление дисками" (нажмите ПКМ по пуску), потом откройте свойства диска (нажмите ПКМ по самому диску, а не по разделам, там где написано "Диск 0" или "Диск 1"), откройте вкладку "Тома" и там посмотрите стиль раздела. Если стиль раздела "MBR", то это плохо
  * Чтобы конвертировать MBR в GPT в Windows без потери данных нужно воспользоваться сторонними программами. Насколько мне известно, конвертировать разметку из MBR в GPT без потери данных позволяют эти программы "Paragon Hard Disk Manager", "AOMEI Partition Assistant" и "MiniTool Partition Wizard" и, возможно, какие-нибудь еще утилиты. К сожалению, только платные версии этих программ позволяют произвести конвертацию. Лично я воспользовался "MiniTool Partition Wizard" и все конвертировалось без потери данных. Так же после выключения нужно в настройках BIOS/UEFI переключить режим загрузки с CSM (Legacy) на UEFI.

## Настройка BIOS / UEFI

Эти параметры очень важны

* Сбросить к заводским настройкам
* Включить UEFI режим
* Включить AHCI
* Отключить Secure Boot
* Установить VideoRAM в 64 МБ


## Создание загрузочной флешки

1. [Скачиваем BootDiskUtility](http://cvad-mac.narod.ru/index/bootdiskutility_exe/0-5)
2. Скачиваем [специальный образ](https://applelife.ru/threads/bdu-macos-i-clover-iz-windows-izgotovlenie-zagruzochnoj-flehshki.37189/page-57#post-557498) macOS Mojave 10.14.6 для BDU (это файл с расширением .hfs, возможно он будет находится в архиве)
3. Вставляем флешку размером 8 ГБ или больше (в любой порт)
4. В BDU

   * Открываем Options -> Configuration
   * Нажимаем "Check Now" и ждем пока не обновится строка рядом
   * Все остальное оставляем поумолчанию
   * Нажимаем "Ок"
   * Выбираем флешку и нажимаем кнопку "Format" и подтверждаем действие

     > Когда флешка отформатируется на ней создадутся 2 раздела: ESP и пустой раздел. На ESP уже будет загрузчик Clover, распакованный из ISO образа из репозитория на Sourceforge

   * Выбираем второй раздел (возможно надо будет нажать на "+" слева от флешки), обычно он находится в самом низу
   * Нажимаем кнопку "Restore" и выбираем файлы 5.hfs
   * Ждем когда полоса прогресса записи образа в BDU дойдет до конца
   * Готово

Так же есть гайд с картинками в pdf файле. Этот мини мануал называется "BDU_FAQ_STARCOM" и [прикреплен к этому](https://applelife.ru/threads/bdu-macos-i-clover-iz-windows-izgotovlenie-zagruzochnoj-flehshki.37189/page-57#post-557498) сообщению.

## Настройка загрузочной флешки

После создания флешки с загрузчиком Clover и самой системой MacOSX, нужно настроить флешку, а точнее загрузчик, потому что просто так MacOSX на компьютер не от Apple не установится из-за отсутствия некоторых различий в оборудовании между Apple компьютерами и обычными ПК или ноутбуками.

### Используемые драйвера

[Список драйверов](/docs/RUS/Configuring/InstalledDrivers.md)

### Настройка файла config.plist

[Инструкция по настройке конфига](/docs/RUS/Configuring/CustomizingConfig.md)

### Используемые кексты

[Инструкция по использованию кекстов](/docs/RUS/Configuring/InstalledKexts.md)

## Установка hackintosh macOS Mojave 10.14.6

Стандартная установка MacOSX:

1. Если вы будете устанавливать MacOSX один диск с Windows и диск размечен в GPT, то вам нужно заранее отделить раздел для установки MacOSX, но если дик размечен в MBR, то нужно его конвертировать в GPT или купить отдельный для установки MacOSX. Подробнее читайте в разделе "Предустановочная подготовка"
2. Если диск вам не нужны файлы, которые находятся на диске, на который вы будете устанавливать MacOSX, то следуем дальше
3. После настройки загрузочной флешки, вставляете ее в компьютер, перезагружаетесь и заходите в BIOS/UEFI или отдельное подменю. Вам нужно выбрать загрузку с вашей флешки и при этом при выборе флешке в строке с ее полным названием, скорее всего, должно быть слово "UEFI"
4. Загружаетесь с флешки и попадаете в GUI меню загрузчика Clover. После этого сразу нажмите какие-нибудь стрелки на клавиатуре, чтобы сбросить таймер (этот таймер можно отключить в config.plist)
5. Переключаетесь стрелками, выбираете пункт "Boot macOS Install from ..." и нажимаете Enter
6. Ждете, пока загрузится окно с выбором языка и выбираете язык
7. В верхней панели (нужно подвести курсор к верху экрана) или в появившемся окне открываете Дисковую утилиту
8. Нажимаете на кнопку "Вид" в левом верхнем углу окна (или в верхней панели) и переключаете вид на "Показать все устройства"
9. Выбираете ваш диск и нажимаете  кнопку "Стереть". Разбить  диск на разделы можно будет и после установки MacOSX также в Дисковой утилите
   * Имя: можно выставить любое, но, наверное, лучше английскими буквами
   * Формат: выбираете APFS
   * Схема: Схема разделов GUID 
10. Закрываете дисковую утилиту и выбираете "Установить macOS"
11. Выбираете раздел, который вы создали в Дисковой утилите. На него и будет устанавливаться. Нажимаете установить
12. Процесс может длиться не столько же минут, сколько указано на экране. Раз в несколько минут шевелите курсор, чтобы компьютер не ушел в режим сна
13. Когда компьютер начнет перезагружаться вам нужно начать загрузку с вашей загрузочной флешки и в GUI меню Clover нужно выбрать пункт "Boot macOS from ..." и нажимаете Enter (вместо трех точек будет название раздела, которое вы написал в Дисковой утилите)
14. Дальше производите манипуляции по первоначальной настройке вашего hackintosh компьютера и настраиваете вашу учетную запись macOS. Желательно все вводить на английском языке. Переключить раскладку (язык) можно с помощью комбинация клавиш Ctrl+Пробел, либо Alt+Пробел, либо Win+Пробел либо мышкой поменять в правом верхнем углу окна. Все дальнейшие настройки можно будет изменить в любой момент в Системных настройках, так что не бойтесь ошибиться

## Настройка macOS

Ниже представлены самые важные, на мой взгляд, пункты, которые нужно включить на большинстве хакинтошей после установки системы.

1. В терминале вводим `sudo spctl --master-disable`, чтобы отключить Gatekeeper и устанавливать приложениея не только из MacAppstore
2. Включить TRIM для SSD дисков с помощью команды в терминале `sudo trimforce enable`
3. Перекинуть файл `MyKeyboardBundle.bundle` в `/Library/Keyboard Layouts` и в `/System/Library/Keyboard Layouts` или сделать этот файл с помощью программы Ukelele и [гайда](https://www.youtube.com/watch?v=Ll6UGWGSSv8). Потом перейти в "Системные настройки", "Клавиатура", "Источники ввода" и добавить новую раскладку и удалить Русскую раскладку.
4. [Установить Clover](https://sourceforge.net/projects/cloverefiboot/) и настроить его перед установкой (слева на одном из этапов будет кнопка "Настроить")
   - Обязательно поставить галочки в первых двух пунктах - "Установить только для UEFI" и "Установить на ESP"
   - Если Clover на вашей флешке настроен, то остальные галочки можно не менять, а если Clover на флешке не настроен, тогда можете настроить его в этом меню
   - Примонтируете раздел EFI вашего диска и флешки, если они не примонтированы
   - Все файлы и папки с EFI флешки копируете на EFI диска с заменой и перезагружаетесь

Полный список настроек, который я применяю, описан в соответствующем файле [**Настройка MacOSX**](/docs/RUS/Configuring/ConfiguringMacOSX.md)

### Установка Windows

[Описание нюансов установки Windows рядом с Hackintosh](/docs/RUS/AboutWindows/InstallWindows.md)

### Настройка Windows

В качестве настройки Windows 10 для работы вместе с macOS мне понадобилось настроить синхронизацию времени и для удобства сделать раздел под файлы с файловой системой ExFat. Про ExFat раздел под файлы я писал выше в инструкции по установке Windows.

Про синхронизацию времени можно почитать и найти прямые руководства там:

* https://appstudio.org/faq/3068.html
* https://www.tonymacx86.com/threads/fix-incorrect-time-in-windows-osx-dual-boot.133719/

## Установка программ

### Программы первой необходимости

Если у вас hackintosh, а не настоящий Apple компьютер, то рекомендую, чтобы эти программы всегда были установлены в вашей системе. Эти программы входят в состав программ из списка Hackintosh Tools, но они являются наиболее необходимыми.

1. Clover Bootloader https://sourceforge.net/projects/cloverefiboot/
2. CloverConfigurator https://mackie100projects.altervista.org/download-clover-configurator/
3. Plist Edit Pro https://www.fatcatsoftware.com/plisteditpro/

### Hackintosh программы

Эти программы программы используются в основном для настройки Hackintosh, как рабочий инструмент macOS они вам вряд ли пригодятся. Часть этих программ желательно оставить в вашем hackintosh на тот случай, если что-то пойдет не так и система начнет "выпендриваться", ведь не все могут определить насколько правильно и насколько полноценно настроен ваш hackintosh.

[Hackintosh tools](/docs/RUS/ProgramsList/HackintoshTools.md)

### Программы-фреймворки для работы других программ

Эти программы нужны для работы других программ или скриптов. В основном это используется разными разработчиками. Если они вам понадобятся, то вы и без этого списка будете знать, какая программа вам нужны.

[Other Software](/docs/RUS/ProgramsList/OtherSoftware.md)

### Устанавливаемое ПО

Большинство прогамм из этого списка я использую в повседневной работе. Естественно, список не самый полный, смотрите и решайте по своим потребностям.

[Regular Software](/docs/RUS/ProgramsList/RegularSoftware.md)

## Тестирование в бенчмарках

Список тестов, их описание и результаты представлены в отдельном файле.

[Результаты тестирования в бенчмарках](/docs/RUS/BenchmarksTesting/benchmark_testing.md)

## Остальное

### Обои

Вы можете найти замечательные обои на рабочий стол на сайте https://unsplash.com/

### Темы на форумах

В этих темах я опубликовал информацию о настройке и установке hackintosh для этого системного блока

https://www.insanelymac.com/forum/topic/339843-guide-dell-vostro-3267-skylake-i5-6400-10146/

https://applelife.ru/threads/sistemnyj-blok-dell-vostro-3267-skylake-i5-6400.2944306/