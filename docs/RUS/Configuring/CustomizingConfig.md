### Настройка файла config.plist

Здесь описано то, как я изменил чистый конфиг, который идет вместе с Clover ревизии 5033.

Вы можете скачать образ диска с данным конфигом с официальной страницы загрузчика Clover: https://sourceforge.net/projects/cloverefiboot/files/Bootable_ISO/ Для распаковки .lzma и .tar файлов в Windows лучше используйте [7zip](https://www.7-zip.org/) или [Bandizip]([https://bandisoft.com/bandizip](https://www.bandisoft.com/bandizip))

Вот, что я изменил в своем файле [config.plist](/CLOVER/config.plist):

```xml
// TODO
```

Это не рабочий config.plist. Выше я привел только те пункты, которые были изменены по сравнению с родным конфигом из ISO образа Clover ревизии 5033.

Такой SMBIOS подойдет для загрузки, но потом лучше сгенерировать его пункты полностью, например с помощью Clover Configurator.

Так же вам не помешает прочитать книгу "[Clover цвета хаки](https://sourceforge.net/projects/cloverefiboot/files/Documents/)", чтобы понимать какие пункты в config.plist за что отвечают.
