- В данном репозитории содержатся только настроечные/установочные скрипты и
дерево каталогов postgresql-10. Поэтому перед сборкой deb-пакета
необходимо скомпилировать postgresql из исходных текстов:

git-export <путь к директории сборки>

export PG_INST_DIR=<путь к директории сборки>/usr

./configure --prefix=$PG_INST_DIR/lib/postgresql/10 \

--datarootdir=$PG_INST_DIR/share/postgresql/10 \

--localedir=$PG_INST_DIR/share/locale \

--docdir=$PG_INST_DIR/share/doc

make (или make world, но у меня так не собиралось)

make install (или make install-world, если до этого собрали make world)

mv $PG_INST_DIR/usr/lib/postgresql/10/lib/libpq.\* $PG_INST_DIR/usr/lib/

Если не собирали world, придётся ещё набрать:

make -C contrib all

make -C contrib install

- Перед установкой пакета необходимо установить пакеты debconf и init-system-helpers


- Во время установки скрипт postinst использует утилиту ucf. В ОС
Эльбрус, в пакете ucf_3.0030-u2, по какой-то причине, среди контрольных
файлов отсутствует файл templates, при этом в самом скрипте ucf, в строке 632 к нему есть обращение: db_x_loadtemplatefile "$(dpkg-query --control-path ucf templates)" ucf

Если файл /var/lib/dpkg/info/ucf.templates отсутствует в системе, то скрипт ucf прекращает свою работу на строке 632 с ошибкой 10.

В связи с этим, перед установкой пакета следует переустановить пакет ucf на правильный, например ucf_3.0036_all.deb
