# Симисторная плата

## Общие сведения
Материнская плата предельно упрощена. Имеет на себе только слоты для краевых разъемов и обеспечивает дочерние платы питанием и шиной передачи данных.
Должна быть фильтрация питания, защита от перенапряжения, пониженного напряжения, обратной полярности.
Так же должна быть индикация наличия питания на материнской плате

## Габаритные размеры
????

## Коннекторы и распиновка
Силовые коннекторы 230V должны иметь запас по току минимум в 2 раза от рассчётных значений
  
## Питание
Питание 230V заводится на каждую симисторную плату отдельно (предпочтительно выполнить в виде двухпроводной медной шины с проводником не менее 1.5 кв.мм) 
Питание управляющей части заводится с внешнего блока питания через коннектор материнской платы 

## Шина данных
Шина представляет собой RS485 с одним лишь дополнением. Используется дополнительно линия RTS, по одной линии на каждый дочерний модуль, за исключением "процессорного модуля"
На "процессорном модуле" количество RTS линий соотвествует количеству возможных дочерних модулей исполнительных устройств, например 16. На "процессорном модуле"
линии RTS - входы. На дочерних - выходы.

## Режим работы нагревательного элемента
Режим работы нагревательного элемента представляет собой нагрев в течение 2 минут на высокой мощности с дальнейшей работой в режиме поддержания температуры (5-10% мощности) в течение 20 минут.

## Дополнительно
- Помехи от коммутации напряжения питания нагревательных элементов не должны влиять на работу других узлов платы управления.
- Силовая шина должна проходить через датчик тока, сигнал с которого должен обрабатываться управляющим контроллером платы
- Свободное место слоев платы должно быть покрыто медной фольгой и подтянуто на землю.
- На печатной плате должны быть обозначены в виде примитивов с нумерацией, все микросхемы и основные элементы. Все разъемы должны быть пронумерованы с обозначением первой ноги. Ножки и разъемы эл. питания должны быть подписаны с указанием уровней напряжений.
- Печатная плата должна быть покрыта защитной маской со всех сторон.
- Первичную трассировку проводов имеет смысл провести до начала разводки плат, дабы исключить коллизии проводов и чтоб появилось понимание оптимального расположения коннекторов на платах.
- Все входы-выходы должны иметь защиту от перенапряжения, обратной полярности, статики и т.п.(см IEC 61131 и прочие посказки)
