- В данном репозитории содержатся только настроечные/установочные скрипты и
дерево каталогов postgresql-10. Поэтому перед сборкой deb-пакета
необходимо скомпилировать postgresql из исходных текстов:

cd <каталог исходных текстов>

./configure \

--prefix=/usr/lib/postgresql/10 \

--datarootdir=/usr/share/postgresql/10 \

--localedir=/usr/share/locale \

--docdir=/usr/share/doc \

--enable-nls

make world

make install-world DESTDIR=<путь к директории сборки>

cd <путь к каталогу pg-inst>

./git-export <путь к директории сборки>

- Перед установкой пакета необходимо установить пакеты debconf, init-system-helpers, а так же переустановить пакет ucf_3.0030-u2 на пакет ucf_3.0030_all. Причина в том, что во время установки скрипт postinst использует утилиту ucf. В ОС Эльбрус, в пакете ucf_3.0030-u2, по какой-то причине, среди контрольных
файлов отсутствует файл templates, при этом в самом скрипте ucf, в строке 632 к нему есть обращение:

db_x_loadtemplatefile "$(dpkg-query --control-path ucf templates)" ucf

Если файл /var/lib/dpkg/info/ucf.templates отсутствует в системе, то скрипт ucf прекращает свою работу на строке 632 с ошибкой 10.

В связи с этим, перед установкой пакета следует переустановить пакет ucf на правильный, например ucf_3.0030_all.deb
