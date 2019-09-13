## Настройка USB портов

Я слышал, что если на хакинтоше не настроены USB порты, то могут быть разные проблемы, например, такие как

* Отсутствие распознавания USB3 портов
* Работа USB3 портов со скоростью USB2 портов
* Медленная скорость зарядки гаджетов по USB
* *Возможно и на что-то еще влияют не настроенные USB порты в хакинтоше*

---

Как проверить нужно ли настраивать USB порты? Я не знаю. Я слышал, что некоторые люди пробовали заряжать iPad от USB портов на Hackintosh и он заряжался медленно, а потом они как-то настроили свой Hackintosh и iPad стал заряжаться быстрее (возможно это не связано с этой настройкой USB портов). Так же вы можете попробовать узнать скорость чтения и записи на флешку, которая поддерживает USB 3, подключив ее к USB 3 порту и протестировав с помощью программы Blackmagic Disk Speed Test или AJA System Test Lite. У моей USB 3.0 флешки после настройки USB портов, программа Blackmagic Disk Speed Test показывала скорость чтения примерно 12-15 MB/s, а скорость записи 90-100 MB/s.

---

Возможно на вашей материнской плате неподдерживаемые USB 3 порты, тогда, перед их настройкой вам нужно сделать так, чтобы macOS их поддерживала. Для этого, обычно, используется кекст [XHCI-unsupported](https://github.com/RehabMan/OS-X-USB-Inject-All) (вам нужно будет скачать весь репозитория, чтобы получить этот кекст).

Возможно на вашей материнской плате очень много USB портов (вроде бы, USB 3 порты ОС распознает не как 1 порт, а как несколько, но я точно не знаю), но macOS не распознает больше 15 USB портов (я не знаю почему). Чтобы это исправить, вам нужно будет использовать "патч на лимит USB портов для Mojave" [ссылка 1](https://ihackline.com/архив/sg-usb-patcher/) [ссылка 2](https://androidp1.ru/zavod-i-nastrojka-usb-portov-na-hakintosh/).

Если у вас не много USB портов и все они поддерживаются в macOS, то для ленивой настройки USB портов достаточно просто поместить кекст [USBInjectAll](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/) в папку `/EFI/CLOVER/kexts/Other` и перезагрузить ПК.

---

После установки hackintosh и основной ее настройки, я не заметил каких-то проблем, связанных с USB портами, но я никак не тестировал их и не пытался выяснить, есть ли какие-то с ними проблемы. Тем не менее для уверенности я добавил кекст [USBInjectAll](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/) в `/EFI/CLOVER/kexts/Other`.

Мой знакомый посоветовал мне настроить USB порты правильным способом, создав кекст USBPorts и несколько SSDT файлов. Разницу между этим способом и просто использованием кекста [USBInjectAll](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/) я не заметил (на этом системном блоке).

Если вы все же хотите попробовать настроить USB порты правильно, то вы можете сделать это вручную: https://www.tonymacx86.com/threads/guide-10-11-usb-changes-and-solutions.173616/ или в полуавтоматическом режиме с помощью программы [Hackintool](https://www.insanelymac.com/forum/topic/335018-hackintool-v280/).

Я воспользовался программой Hackintool.

Для начала вам нужно либо пропатчить DSDT файл, либо вписать в конфиг соответствущие патчи, чтобы загрузчик сам пропатчил DSDT во время загрузки.

Я действовал по [этой](https://translate.google.com/translate?js=n&sl=auto&tl=en&u=https://blog.daliansky.net/Intel-FB-Patcher-tutorial-and-insertion-pose.html) инструкции от Daliansky. Для настройки USB, вписал в config эти патчи. Я не знаю, нужны ли они все, но мне кажется, они не взаимоисключающие и их можно применять вместе.

* XHCI to XHC
* XHCI1 to XHC
* H_EC to EC
* ECDV to EC
* EHC1 to EH01
* EHC2 to EH02
* EC0 to EC

КАРТИНКИ !!!!!

Потом я перезагрузил ПК, запустил Hackintool и открыл вкладку USB. Там отображался список/таблица USB портов и некоторые из них (половина или большая часть) были подсвечены зеленым цветом. Потом я взя флешку, которая поддерживает передачу данных по стандарту USB 3.0, вынул клавиатуру и мышку из ПК и начал вставлять эту флешку во все USB порты по очереди. Потом я взял флешку, которая поддерживает только USB 2.0 и так же начал вставлять и вынимать ее во все USB порты.

Потом я удалил из списка/таблицы все строки, которые не были подсвечены зеленым цветом (выделяем строку и внизу нажимаем на круглую кнопку с минусом). Далее, все порты, у которых было название SS и скорость передачи 5 Gbps я пометил в таблице как *USB 3* порты (в колонке Connector). Строки, в которых были IOUSBHostDevice (скорее всего это bluetooth) и USB2.0-CRW (скорее всего это Card Reader) я пометил как *Internal* порты. А все остальные я пометил как *USB 2* порты (все в той же колонке Connector).

Потом я нажал на кнопку *Export* (она там же внизу), указал папку и программа сгенерировала несколько файлов:

* USBPorts.kext
* SSDT-EC.aml
* SSDT-UIAC.aml
* SSDT-USBX.aml
* config.plist

В сгенерированном файле config.plist были только патчи, о которых я говорил выше. Если те патчи вы применили, то этот plist файл можете просто удалить.

SSDT файлы нужно переместить в папку `/EFI/CLOVER/ACPI/patched`, а USBPorts.kext нужно переместить в `/EFI/CLOVER/kexts/Other` и после этого удалить кекст USBInjectAll.kext.

После этого нужно перезагрузить ПК и, теоретически, у вас будут правильно работать USB порты в Hackintosh, но, как я уже говорил, я на этом ПК не заметил никаких изменений.

---

Вот список патчей для config.plist, которые я использовал:

```xml
<dict>
	<key>Comment</key>
	<string>Rename XHCI to XHC (USB)</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>WEhDSQ==</data>
	<key>Replace</key>
	<data>WEhDXw==</data>
</dict>
<dict>
	<key>Comment</key>
	<string>Rename XHC1 to XHC (USB)</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>WEhDMQ==</data>
	<key>Replace</key>
	<data>WEhDXw==</data>
</dict>
<dict>
	<key>Comment</key>
	<string>Rename H_EC to EC (USB Power)</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>SF9FQw==</data>
	<key>Replace</key>
	<data>RUNfXw==</data>
</dict>
<dict>
	<key>Comment</key>
	<string>Rename ECDV to EC (USB Power)</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>RUNEVg==</data>
	<key>Replace</key>
	<data>RUNfXw==</data>
</dict>
<dict>
	<key>Comment</key>
	<string>change EHC1 to EH01</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>RUhDMQ==</data>
	<key>Replace</key>
	<data>RUgwMQ==</data>
</dict>
<dict>
	<key>Comment</key>
	<string>change EHC2 to EH02</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>RUhDMg==</data>
	<key>Replace</key>
	<data>RUgwMg==</data>
</dict>
<dict>
	<key>Comment</key>
	<string>change EC0 to EC</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>RUMwXw==</data>
	<key>Replace</key>
	<data>RUNfXw==</data>
</dict>
```

