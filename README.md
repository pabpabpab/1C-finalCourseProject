Алексей Постников, junior 1С-программист.
Telegram: @leha111, email: pax75@yandex.ru

В данном репозитории мой финальный проект на курсе "1С-разработчик с нуля до PRO" - Автоматизация торгового предприятия (подсистемы: Закупки, Продажи, Казначейство, Зарплата, Бухгалтерия, Администрирование).

<br/>
Отзыв преподавателя на одну из моих практических работ на курсе
> код не похож на код новичка

[blob/main/notBeginner.png](https://github.com/pabpabpab/1C-finalCourseProject/blob/main/notBeginner.png)

----------------------------------------------------------
----------------------------------------------------------
<br/>


Наиболее содержательные с точки зрения программирования процедуры и функции в модулях форм документов: 

1) Документ УстановкаКурсовВалют

- ЗагрузитьВсеВалютыНаСервере(); <br/>
Загрузить в ТЧ все валюты кроме основной, и их последний курс и кратность 
(для удобства пользователя при установке курсов валют);

-  КурсыВалютВалютаПриИзменении(); <br/>
 Для контроля вводимых пользователем данных, 
 если попытка ввести валюту которая уже есть в ТЧ или ввести основную валюту, то сообщить о неправильных действиях.
 При правильных действиях показать последние данные (курс и кратность) по выбранной валюте;
----------------------------------------------------------

2) Документ УстановкаЦенНоменклатуры
    
- ЗагрузитьНоменклатуруСНеустановленнойЦенойНаСервере(); <br/> 
Загрузить в ТЧ номенклатуру для которой цена еще не устанавливалась,     
можно указать отбор "ВидНоменклатуры" и нужно указать отбор "ВидЦены";
       
- ЗагрузитьНоменклатуруСЦенойНаСервере();<br/>
Загрузить в ТЧ номенклатуру и ее цену установленную в предыдущий раз (для удобства пользователя), 
можно указать отбор "ВидНоменклатуры" и нужно указать отбор "ВидЦены"; 

- ТоварыНоменклатураПриИзменении(Элемент);<br/>
Для контроля вводимых пользователем данных, 
если попытка ввести номенклатуру которая уже есть в ТЧ, то сообщить о неправильных действиях;
----------------------------------------------------------

3) Документ ЗаказПоставщику 

- ЗагрузитьНоменклатуруНаСервере();<br/>
Загрузить номенклатуру в ТЧ с указанной ценой (для удобства пользователя), 
можно указать отбор "ВидНоменклатуры";

- УдалитьНеотмеченныеНаСервере();<br/>
Удалить неотмеченные флагом "КОформлению" записи из ТЧ;

-  ПересчитатьЦеныПриИзмененииВалюты(НоваяВалюта);<br/>
При изменении валюты документа пересчитываются цены и стоимость товаров и сумма докуммента по курсу новой валюты.
(Запрос - Соединение данных из ТЧ документа в виде временной таблицы и РС ЦеныНоменклатуры.СрезПоследних);

- ВыгрузитьНаСервере(ПутьКФайлу);<br/>
Выгрузить товары в CSV-файл.
В csv-файл выгружаются строки (у которых установлен флаг КОформлению) из ТЧ документа;
----------------------------------------------------------

4) Документ ПоступлениеТоваров

- ЗагрузитьНаСервере(ПутьКФайлу);<br/>
Загрузить данные из csv-файла (Формат файла Номенклатура;Цена;Количество;Стоимость;Штрихкод). 
В последней строке документа - Валюта документа;

- ПоказатьШтрихкодыЭлементаНоменклатуры();<br/>
Открыть модально произвольную форму РС Штрихкоды (по имени ВсеШтрихкодыЭлементаНоменклатуры),
в которую параметром передать выбранный в ТЧ элемент номенклатуры. 
В самой открываемой программно форме есть реквизит Штрихкоды типа ДинамическийСписок,
для которого составлен произвольный запрос - все штрихкоды в регистре в разрезе партий для переданного элемента номенклатуры. 
При двойном щелчке на строке ТЧ в модальной форме в первую форму возвращается значение штрихкода;

- ЗаполнитьШтрихкодыНаСервере();<br/>
Заполнить колонку Штрихкод в ТЧ, данными из РС Штрихкоды;
----------------------------------------------------------

5) Документ ЗаказКлиента

- ПолучитьТаблицуЗначенийИзCSVФайла(ПутьКФайлу);<br/>
Загрузить данные из csv-файла в ТаблицуЗначений. Формат csv-файла - Номенклатура;Количество. 
Для того чтобы потом в процедуре ЗагрузитьНаСервере() присоединить цены из РС и загрузить все в ТЧ;

- ЗагрузитьНаСервере(ПутьКФайлу);<br/>
Загрузить данные в ТЧ из csv-файла.
Сначала загружаются данные из csv-файла в ТаблицуЗначений, затем создается запрос - связь двух таблиц, 
первая таблица - внешний источник - ТаблицаЗначений, вторая таблица - РегистрСведений.ЦеныНоменклатуры.СрезПоследних; 
----------------------------------------------------------

6) Документ РеализацияТоваров

- ЗаполнитьЦеныНаСервере();<br/>
Загрузить цены из РС ЦеныНоменклатуры в ячейки строк ТЧ без цены;


В модуле объекта документа РеализацияТоваров:

- ПроверитьЦенуВТабличнойЧасти(Отказ);<br/>
Перед записью документа прверить цены в ТЧ: если цена номенклатуры в ТЧ документа меньше цены розничной в РС ЦеныНоменклатуры, 
то установить Отказ в Истину;
----------------------------------------------------------

7) Документ УстановкаОкладов

- ЗагрузитьСотрудниковНаСервере();<br/>
Загрузить сотрудников в ТЧ с установленным в предыдущий раз окладом из РС ОкладыСотрудников;
----------------------------------------------------------

8) Документ НачислениеЗарплаты (модуль объекта)

- ПолучитьСотрудниковИзТЧНачислений();<br/>
Получение одним запросом уникальных сотрудников из двух таб частей документа,
чтобы потом использовать для расчета НДФЛ со всех начислений;
----------------------------------------------------------

А также вводы на основании, и проводки в регистры сведений, накопления, бухгалтерии, расчетов.

----------------------------------------------------------
----------------------------------------------------------
<br/>


Процедуры и функции в общих модулях:

1) Модуль ТабЧастьДокументаКлиент

- ЭлементУжеСуществует(ТабЧасть, НаименованиеКолонки, ИскомыйЭлемент);<br/>
Используется в модулях формы некоторых документов при добавлении или изменении в ТЧ документа элемента справочника 
для контроля уникальности его в ТЧ; 


- ВыбратьВсе(ТабЧасть, ПолеВыбораНаименование);<br/>
Если все чекбоксы поля выбора типа Булево в одинаковом состоянии, 
то возвести их в противоположное их состояние, иначе возвести все чекбоксы в Истину;
-----------------------------------------------

2) Модуль ВалютыСервер

- ПолучитьКурсВалюты(Валюта, Дата); <br/>
Получает из РС данные и возвращает структуру со свойствами Курс и Кратность, значением которых является 
курс и кратность указанной в параметрах валюты на указанную дату;
-----------------------------------------------

3) Модуль РеквизитыСправочниковСервер

- ПолучитьИменаИСинонимыСправочников();<br/>
Возвращает объект типа СписокЗначений, содержащий свойства метаданных всех справочников, 
а именно Имя и Синоним (функция используется для выбора справочника в обработке загрузки и выгрузки в файл);  

- ПолучитьМассивИменРеквизитов(ИмяСправочника);<br/>
Возвращает массив имен реквизитов справочника (функция используется в обработке загрузки и выгрузки справочника в файл из файла);  

- ПолучитьПредставлениеОбъекта(ИмяСправочника);<br/>
Возвращает представление объекта справочника (вида СтрокаБезПробелов), если оно существует, 
иначе возвращает имя справочника (функция используется в обработке загрузки и выгрузки справочника в xml-файл из файла);  
-----------------------------------------------

4) Модуль ГруппыИЭлементыСправочниковСервер

- ПолучитьПутьЭлемента(РодительЭлемента);<br/>
Возвращает полный путь родителя элемента или группы (зная родитель элемента справочника, можно добраться до корня справочника и составить полный путь вложенности до места, где находится нужный элемент);

- ПолучитьСсылкуНаГруппуВСправочникеИначеСоздатьГруппу(ИмяСправочника, ПолноеИмяГруппы);<br/>
Возвращает ссылку на вложенную группу, полное имя (полный путь) которой указывается в параметре ПолноеИмяГруппы, 
если группа не существует, то она (и вся иерархия выше) создается;

- ЗаполнитьЭлементСправочника(НовыйЭлемент, НаборРеквизитов, СправочникСсылка);<br/>
Заполняет реквизиты элемента справочника значениями из структуры, 
где ключи - имена реквизитов справочника, значения - значения реквизитов,
при этом нет привязки к конкретному справочнику (попытка написать общую процедуру);
-----------------------------------------------

5) Модуль ЦеныНоменклатурыСервер

- ПолучитьСправкуПоЦенамЭлементаНоменклатуры(ЭлементНоменклатуры);<br/>
Возвращает текст справки об установленных ценах (всех видов цен) для элемента номекнлатуры;
-----------------------------------------------

6) Модуль ЗагрузкаТабЧастиИзCSVФайлаСервер

- Загрузить(ТабЧасть, ПутьКФайлу, УказанаВалютаВПоследнейСтрокеФайла = Ложь, СвойствоВалютаРеквизитаФормы = Null);<br/>
Загрузка данных (вида, например: Штрихкод;Номенклатура;Цена;Количество;Стоимость) 
из CSV-файла, в котором последняя строка файла это Наименование валюты, в табличную часть;
-----------------------------------------------

7) Модули выгрузки и загрузки справочников в/из CSV и XML файлы.

----------------------------------------------------------
----------------------------------------------------------
<br/>

Обработки:

1) Обработка СозданиеДокументаПоступленияИлиРеализацииТоваров

- ЗагрузитьОстаткиТоваровНаСервере();<br/>
В ТЧ формы загружаются номенклатура и количество из РН ТоварыНаСкладах (вирт таблицы ТоварыНаСкладахОстатки), 
и цены указанного в форме вида из РС ЦеныНоменклатуры;

- СоздатьДокументНаСервере();<br/>
Заполнение реквизитов объекта типа ДокументОбъект реквизитами формы обработки и запись документа, 
предварительно проверка на роль пользователя на создание документа РТУ или ПТУ;
----------------------------------------------------------

2) Обработка ВыгрузкаЗагрузкаСправочника в/из файла в csv или xml формате

----------------------------------------------------------
----------------------------------------------------------
<br/>

Отчеты:

ОстаткиТоваровНаСкладах, ПродажиОстаткиЦены, ВзаиморасчетыСПоставщиками, ВзаиморасчетыСПокупателями, НачисленияСотрудникам
