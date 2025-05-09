# Плата фото\светодиодов

## Общие сведения
На плате с одной стороны располагаются линзированные или иные типы светодиодов обеспечивающих плотный направленный пучок света.
С противоположной стороны - фоточувствительные элементы для приема сигнала с соседней платы оптических дачиков.
Плата имеет разъем для подачи питания и передачи сигнала, предлагаю использовать разъем типа 10P10C.

## Назначение платы
Стоит в разъёмах между ячейками выдачи теста (в одну сторону светит, с другой
стороны принимает)
Обнаруживает, поднялось ли тесто до нужной высоты (перекрыло ли тесто оптопару)
Управляется (получает питание, отдаёт сигнал) дискретной платой
Мониторит точки независимо (этот участок уже перекрыт, этот ещё нет)

## Функции
[[../requirements.md#Общие требования для всех плат]]

[[../requirements.md#Общие требования для дочерних плат]]

1. Нагрев любого участка платы не выше 60 градусов
   Плата будет в пластиковом "чехле", чехол прилегает в упор 
   нагреем выше, пластик расплавится
2. Рабочий цикл: ~ 30 секунд, с интервалом не менее 1 минуты
   (во время выдачи из ячейки включили свето и фото, снимаем сигналы)
3. Допускает отключение питания во время простоя
4. Контур светодиодов
   1. Напряжение: 22..25 В
   2. Потребляемый ток: \<= 0.5 А
5. Контур фототранзисторов
   Независимые дискретные сигналы, 24 В
   1. Питание
      1. Напряжение: 22..25 В
      2. Потребляемый ток: \<= 0.5 А
   2. Время реакции: \< 1 мс
      (время от перекрытия тестом датчика до фронта на коннекторе)
   3. Напряжение сигнала 
      1. Нет теста: \>= 22 В
      2. Есть тесто: \<= 1 В
5. Независимость контуров
   Должна быть возможность питать контуры диодов и транзисторов независимо друг
   от друга (получать сигналы с транзисторов при отсутствии питания диодов)
7. Интерфейс с дискретной платой
   1. Сигналы с фототранзисторов должны подключаться во входы дискретной платы
   2. Питание светодиодов должно подключаться в один из выходов дискретной платы
   3. Контуры подключаются в разные дискретные платы
      (led\_1.photo подключается в di\_1.in, led\_2.led подключается в
      di\_2.out)

## Пояснения
На одну ячейку теста приходится две платы (одна как источник, вторая как приёмник)
Хотим отключать питание неиспользуемых плат (экономить электричество, не греть
лишний раз)
-> должны уметь питать светодиоды и фотодиоды с разных дискретных плат


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
