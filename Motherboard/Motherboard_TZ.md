# Материнская плата

## Общие сведения
Материнская плата предельно упрощена. Имеет на себе только слоты для краевых разъемов и обеспечивает дочерние платы питанием и шиной передачи данных.
Должна быть фильтрация питания, защита от перенапряжения, пониженного напряжения, обратной полярности.
Так же должна быть индикация наличия питания на материнской плате

## Габаритные размеры
????

## Коннекторы и распиновка
Все краевые разъёмы должны иметь идентичную распиновку.
К плате подводится через 2 соседних коннектора: 
- питание логики
- связь с серверным ПК
- 
## Питание
Питание заводится со стабилизированного блока питания 24В

## Шина данных
Шина представляет собой RS485 с одним лишь дополнением. Используется дополнительно линия RTS, по одной линии на каждый дочерний модуль, за исключением "процессорного модуля"
На "процессорном модуле" количество RTS линий соотвествует количеству возможных дочерних модулей исполнительных устройств, например 16. На "процессорном модуле"
линии RTS - входы. На дочерних - выходы.

## Дополнительно
Все входы-выходы должны иметь защиту от перенапряжения, обратной полярности, статики и т.п.(см IEC 61131 и прочие посказки)
