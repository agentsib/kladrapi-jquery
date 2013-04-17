Primepix КЛАДР
==============

Primepix КЛАДР – виджет jQuery UI для автодополнения адреса при вводе.
Виджет унаследован от $.ui.autocomplete, в качестве источника данных используется сервис [kladr-api.ru] [1]


Опции виджета
-------------

* **key** – ключ для доступа к сервису [kladr-api.ru] [1], по умолчанию равен *null*
* **type** – тип объектов для подстановки, по умолчанию *$.ui.kladrObjectType.REGION*
* **parentType** – тип родительского объекта для ограничения области поиска, по умолчанию *$.ui.kladrObjectType.REGION*
* **parentId** – код родительского объекта для ограничения области поиска, по умолчанию равен *null* - область поиска не ограничивается
* **withParents** – получить объект вместе с родителями, по умолчанию *false*
* **label** *= function( obj, query) { return label; }* – функция для получения подписей, в качестве параметров принимает *obj* – объект КЛАДР, query – текущее значение поля ввода
* **value** *= function( obj, query) { return label; }* – функция для получения подставляемых значений, в качестве параметров принимает *obj* – объект КЛАДР, query – текущее значение поля ввода
* **filter** *= function( array, term ){ return array; }* – функция для фильтрации вариантов подстановки.
* **select** *= function( event, ui ){}* – функция вызываемая при событии выбора пользователем значения для автодополнения, в поле *ui.item.obj* – передаётся выбранный пользователем объект КЛАДР


Перечисление $.ui.kladrObjectType
---------------------------------

* **$.ui.kladrObjectType.REGION**  -  область, регион
* **$.ui.kladrObjectType.DISTRICT**  -  район
* **$.ui.kladrObjectType.CITY**  -  населённый пункт
* **$.ui.kladrObjectType.STREET**  -  улица
* **$.ui.kladrObjectType.BUILDING** -  дом, строение


Формат объекта КЛАДР
--------------------

`````javascript
{
	id: "2900000100000", // Код объекта
	name: "Архангельск", // Название объекта
	zip: "163000",       // Почтовый индекс объекта
	type: "Город",       // Тип объекта (подпись) полностью
	typeShort: "г",      // Тип объекта (подпись) коротко
	parents: [           // Массив родительских объектов
		{
			id: "2900000000000",
			name: "Архангельская",
			zip: null,
			type: "Область",
			typeShort: "обл"
		}
	]
}
`````


Служебная функция $.kladrapi
----------------------------

Функция **$.kladrapi( options, callback )** делает запрос  к сервису [kladr-api.ru] [1], результат возвращает в функцию *callback*. В качестве options принимает объект с параметрами запроса. 

Список возможных параметров:
* **key** – ключ для доступа к сервису
* **regionId** – код региона
* **districtId** – код района
* **cityId** – код города
* **streetId** – код улицы
* **buildingId** – код строения
* **query** – строка для поиска по названию
* **contentType** – тип объекта для поиска
* **withParent** – вернуть объекты вместе с родителями, если 1 то в каждый объект будет добавлено поле *parents* содержащее список объектов-родителей объекта
* **limit** – ограничение количества возвращаемых объектов, по умолчанию равно 2000


Примеры
-------

Простое автодополнение города

`````javascript
$( "input" ).kladr({
	key: 'demo',
	type: $.ui.kladrObjectType.CITY
});
`````

Автодополнение городами из Архангельской области (код КЛАДР 2900000000000)

`````javascript
$( "input" ).kladr({
	key: 'demo',
	type: $.ui.kladrObjectType.CITY,
	parentType: $.ui.kladrObjectType.REGION,
	parentId: "2900000000000"
});
`````


Автодополнение городами России, со сменой подписи при выборе города

`````javascript
$( "input" ).kladr({
	key: 'demo',
	type: $.ui.kladrObjectType.CITY,
	select: function( event, ui ) {
		$( "label" ).text( ui.item.obj.type );
	}
});
`````

Более подробные примеры можно посмотреть в файлах  [example1.html](https://github.com/xescoder/kladrapi-jquery/blob/master/example1.html) и [example2.html](https://github.com/xescoder/kladrapi-jquery/blob/master/example2.html)


[1]: http://kladr-api.ru/        "КЛАДР API"