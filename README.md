- В данном репозитории содержатся только настроечные/установочные скрипты и
дерево каталогов postgresql-10. Поэтому перед сборкой deb-пакета
необходимо скомпилировать postgresql из исходных текстов:

git-export <путь к директории сборки>

cd <каталог исходных текстов>

./configure \

--prefix=/usr/lib/postgresql/10 \

--datarootdir=/usr/share/postgresql/10 \

--localedir=/usr/share/locale \

--docdir=/usr/share/doc

make (или make world, но у меня так не собиралось)

make install DESTDIR=<путь к директории сборки> (или make install-world DESTDIR=<путь к директории сборки>, если до этого собрали make world)

Если не собирали world, придётся ещё набрать:

make -C contrib all

make -C contrib install DESTDIR=<путь к директории сборки>

- Перед установкой пакета необходимо установить пакеты debconf и init-system-helpers


- Во время установки скрипт postinst использует утилиту ucf. В ОС
Эльбрус, в пакете ucf_3.0030-u2, по какой-то причине, среди контрольных
файлов отсутствует файл templates, при этом в самом скрипте ucf, в строке 632 к нему есть обращение: db_x_loadtemplatefile "$(dpkg-query --control-path ucf templates)" ucf

Если файл /var/lib/dpkg/info/ucf.templates отсутствует в системе, то скрипт ucf прекращает свою работу на строке 632 с ошибкой 10.

В связи с этим, перед установкой пакета следует переустановить пакет ucf на правильный, например ucf_3.0036_all.deb
