Плата будет в пластиковом "чехле", чехол прилегает в упор -> нагрев не выше 60 градусов (иначе пластик поплавится)
Рабочий цикл (во время выдачи из ячейки включили свето и фото, снимаем сигналы) ~ 30 секунд, с интервалом не менее 1 минуты
Управляется (получает питание, отдаёт сигнал) дискретной платой
Допускает отключение питания во время простоя

- Плата стоит в разъёмах между ячейками (в одну сторону светит, с другой стороны
принимает)
- Плата подключается в дискретную плату (в одну дискретную одна led плата)
- На одну ячейку приходится две платы (одна как источник, вторая как приёмник)
- Хотим отключать питание неиспользуемых плат
-> должны уметь питать светодиоды и фотодиоды с разных дискретных плат

Общие

# Плата фото\светодиодов

## Общие сведения
На плате с одной стороны располагаются линзированные или иные типы светодиодов обеспечивающих плотный направленный пучок света.
С противоположной стороны - фоточувствительные элементы для приема сигнала с соседней платы оптических дачиков.
Плата имеет разъем для подачи питания и передачи сигнала, предлагаю использовать разъем типа 10P10C.

## Габаритные размеры
????

## Коннекторы и распиновка
Плата подключается IDC шлейфом
  
## Питание
Питание заводится со стабилизированного блока питания 24В (через материнскую плату)

## Шина данных
Шина представляет собой RS485 с одним лишь дополнением. Используется дополнительно линия RTS, по одной линии на каждый дочерний модуль, за исключением "процессорного модуля"
На "процессорном модуле" количество RTS линий соотвествует количеству возможных дочерних модулей исполнительных устройств, например 16. На "процессорном модуле"
линии RTS - входы. На дочерних - выходы.

## Дополнительно
Плата должна иметь транзисторные или иные выходы на полупроводниках, совместимые по уровню сигнала с платой дискретных входов контроллера. Потребление платы не должно превышать 500мА. Плата должна иметь фильтр питания, защиту от повышенного напряжения, индикатор питания а так же иметь защиту выходов.
Количество светодиодов и фотоэлементов - по 8 штук.
Все входы-выходы должны иметь защиту от перенапряжения, обратной полярности, статики и т.п.(см IEC 61131 и прочие посказки)