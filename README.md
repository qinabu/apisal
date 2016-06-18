АПИ Сал
-------

Сервис для создания развёрнутых описаний по маршруту.
Ты ему на вход трэк, записанный с помощью Strava или Run Keeper,
а он тебе текстовое описание с картинками, видео, аудио.

Первая версия работает с трэками в формате gpx.
Работает как набор линуксовых команд. Такой себе пайп.

Pointer перегоняет трэк из инородного формата (gpx)
в набор точек, понятный АПИ Салу.

Парсеры добывают информацию об объектах, расположенных
по ходу следования маршрута, из открытых источников
(wikimapia, wikipedia, foursquare).

Рендереры формируют отчёт в запрошенном формате: xml, html.

Запуск АПИ Сал для формирования отчёта в виде html из gpx-трэка:

```
git clone https://github.com/pik4ez/apisal.git
cd apisal
make
./run.sh
```
