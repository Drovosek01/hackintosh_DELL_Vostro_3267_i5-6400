### Настройка Wifi

В этом системном блоке установлен модуль `DW1707`. Насколько я знаю, это комбинированный Wifi + bluetooth модуль на основе Qualcomm Atheros AR9565. Начиная с какой-то версии macOS, Apple отказалась от поддержки модулей Atheros, поэтому в Mojave необходимо взять элемент, который отвечает за поддержку Atheros модулей из предыдущей версии macOS.

#### Как заставить работать AR9565 в macOS Mojave 10.14.x

Есть несколько способов это сделать, начиная от самого действенного (на мой взгляд) и заканчивая самым автоматизированным. Все эти способы делают примерно одно и то же, только разными путями.

##### Способ 1

1. Необходимо скачать пропатченный кекст `AirPortAtheros40.kext ` для AR9565 для macOS Mojave (например, пользователь **chunnann** выложил всевозможные варианты этого кекста здесь на форуме [insanelymac](https://www.insanelymac.com/forum/topic/312045-atheros-wireless-driver-os-x-101112-for-unsupported-cards/?page=17&tab=comments#comment-2509900))
2. Потом нужно положить `AirPortAtheros40.kext ` внутрь другого кекста - в `/System/Library/Extensions/IO80211Family.kext/Contents/PlugIns ` (скорее всего вам понадобится включить отображение скрытых файлов и папок, например с помощью программы [ShowHiddenFiles](https://gotoes.org/sales/ShowHiddenFilesMacOSX/How_To_Show_Hidden_Files.php))
3. И пересобрать кэш ядра/кэш кекстов (я делал это так - просто запускаем программу [KextUtility]([http://cvad-mac.narod.ru/index/0-4](https://vk.com/away.php?to=http%3A%2F%2Fcvad-mac.narod.ru%2Findex%2F0-4&cc_key=)) и ждем пока кнопка "Quit" станет активной и нажимаем на нее).

##### Способ 2

Используем специальный скрипт специально для модуля AR9565 из темы на [insanelymac](https://www.insanelymac.com/forum/topic/328426-qualcomm-atheros-ar9565-wireless-for-os-x-108-1014/)

> Откройте терминал, построчно копируйте код ниже и после вставки каждой строки нажимайте "Enter"
>
> ```bash
> curl -O https://raw.githubusercontent.com/kalifans/Darwin/Driver/ar956x-drv-osx.tar.gz
> tar zxvf ar956x-drv-osx.tar.gz
> cd ar956x-drv-osx
> ./ar956x-inst.sh
> ```

##### Способ 3

Используем специальную программу для модулей Atheros с форума [insanelymac](https://www.insanelymac.com/forum/files/file/956-atheros-installer-for-macos-mojave-and-catalina/)

##### Способ 4

Используем специальную программу из социальной сети ВКонтакте: https://vk.com/wall-135785821_1712
* AR946x: https://yadi.sk/d/SMSlrmt-BF4sMA
* AR9485: https://yadi.sk/d/Fw-v6EKbTUB8jg
* AR9565: https://yadi.sk/d/STSUoAGqi0MHcA

После применения одного из этих способов нужно перезагрузить ваш компьютер.