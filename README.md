# АПИ Сал

Сервис для создания развёрнутых описаний по маршруту.
Ты ему на вход трек, записанный с помощью Strava или Run Keeper,
а он тебе текстовое описание с картинками, видео, аудио.

Первая версия работает с треками в формате gpx.
Работает как набор независимых утилит. Такой себе пайп.

## Запуск

Простейший способ использования -- запустить веб-сервер и загрузить gpx-трек
через форму с одним полем.

```
git clone https://github.com/pik4ez/apisal.git
cd apisal
make
./run.sh
```

Далее открываем в браузере http://localhost:3000.

## Поддерживаемые форматы треков

Пока вооот в таком формате:

```
<?xml version="1.0" encoding="UTF-8"?>
<gpx version="1.0">
	<name>Example gpx</name>
	<wpt lat="55.749321" lon="37.608839">
		<ele>0</ele>
		<name>API SAL</name>
	</wpt>
	<trk><name>Example gpx</name><number>1</number><trkseg>
		<trkpt lat="55.749321" lon="37.608839"><ele>0</ele><time>2007-10-14T10:09:57Z</time></trkpt>
		<trkpt lat="55.754563" lon="37.612369"><ele>0</ele><time>2007-10-14T10:10:52Z</time></trkpt>
		<trkpt lat="55.756935" lon="37.615224"><ele>0</ele><time>2007-10-14T10:12:39Z</time></trkpt>
		<trkpt lat="55.758528" lon="37.618050"><ele>0</ele><time>2007-10-14T10:13:12Z</time></trkpt>
	</trkseg></trk>
</gpx>
```

## Общая схема работы

* Добываем точки из трека
* Нормализуем трек, убирая большие скопления точек
* Ищем интересующие геообъекты в открытых источниках
* Добавляем текстовые связки между объектами (объекты-лигатуры)
* Отсеиваем неподходящие по формату геообъекты
* Формируем отчёт в требуемом формате

## Типы объектов

* organic -- геообъекты, полученные из внешних источников (wikimapia, foursquare и т. д.)
* legature -- текстовые связки, сгенерированные на основе маршрута и окружающих объектов

## Утилиты, формирующие пайп

### pointer

Pointer перегоняет трек из инородного формата (gpx)
в набор точек, понятный АПИ Салу.

```
-gpx-file string
    GPX file path
```

### smoother

Нормализует трек, избавляясь от слишком близко расположенных точек.

```
-min-distance float
    Min distance between points
```

### parser-wikimapia

Добывает геообъекты, расположенные по ходу следования маршрута, из викимапии.

### parser-foursquare

Добывает геообъекты, расположенные по ходу следования маршрута, из foursquare.

### filter

Отсеивает органические объекты, основываясь на длине заголовка, описания
и количестве фотографий.

```
-d int
    Minimum description length (default 128)
-p int
    Minimum photos per object
-t int
    Minimum title length (default 10)
```

### renderer-html

Создаёт html-файл с указанным шаблоном из набора входящих объектов
и точек маршрута.

```
-o string
    Objects filename
-p string
    Points filename
-t string
    Template filename
```

### renderer-xml

Создаёт xml-файл с объектами маршрута.