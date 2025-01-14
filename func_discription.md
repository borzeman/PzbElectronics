### Функции

**slave_init** - светодиод на плате мигает после инициализации slave-контроллера. (4 пульса по 500мс)

**slave_RTS_updown** - контроллер slave платы после инициализации поднимает линию RTS, после ждёт команды на смену состояния линии и раз в 10 секунд меняет его на противоположное. Дублировать поднятое состояние RTS светодиодом на плате. 

**slave_parad** - дочерние по очереди каждая шлёт своё сообщение в шину. Шлёт, когда видит на шине сообщение предыдущей платы
(плата\_2 ждёт сообщение '111', увидела - шлёт '222', ждёт дальше)
управляющая тогглит диод, когда очередь проходит через неё
(т.е. если всё ок, на управляющей диод мерцает с одинаковой частотой)

**slave_triac_load** - открывает симисторы на 25\50\75\100% в цикле 30 секунд. Дважды

**slave_analog_load** - открывает симисторы на 25\50\75\100% в цикле 30 секунд. Дважды

**slave_analog_readout** - читает\логирует\при запросе отправляет по UART показания датчика тока и термопар

**slave_analog_forced_readout** - при получении команды "r", поднять RTS и отправить на Master текущие логи температуры и показаний датчика тока.

**slave_analog_relay_switcher** - раз в 10 сек переключать состояние реле печи на противоположное.

**slave_digit_load** - контроллер ожидает последовательной сработки всех фототранзисторов, после этого начинает параллельно грузить спирали стенда в течении 60 сек. По истечении 60 сек снять нагрузку со спиралей и зажечь индикаторный светодиод.

**slave_step_load** - ШД вращается в любую сторону до последовательного нажатия на все 4 концевика, потом вращается в другую сторону. Нажать на все концевики последовательно ещё раз - ШД остановится, после этого загорится светодиод.

**slave_step_load_max** - Оба ШД с максимальным ускорением и скоростью совершают рваные движения на протяжении 60 сек, при нажатии на любой из концевиков движение ставится на паузу, при отпускании - возвращается. Зажечь индикаторный светодиод.

**slave_step_load_min** - ШД вращается в любую сторону со скоростью 1 шаг\сек, после нажатия на 1 или 2 концевик - меняет направление вращения, если была хотя бы 1 сработка концевика - спустя 60 сек с момента старта - зажечь индикаторный светодиод. (отладочные GPIO дублируют сработку 3 и 4 концевиков)

**slave_dc_load** - ДПТ 19 сек вращается в одну сторону, 1 сек в другую, затем наоборот и так 3 раза(всего 60 сек). Зажечь индикаторный светодиод.

**slave_dc_load_max** - ДПТ вращается в любую сторону со скоростью 1 шаг\сек, после нажатия на 3 или 4 концевик, идущий к плате ШД - меняет направление вращения, если была хотя бы 1 сработка концевика - спустя 60 сек с момента старта - зажечь индикаторный светодиод. (отладочные GPIO ловят сработку с платы ШД)

**slave_QR_load** - Считывает QR код, поднимает RTS, отправляет содержимое на Master, после получения от мастера сообщения об успешном приёме, зажечь индикаторный светодиод.

**slave_blink** - моргнуть индикаторным светодиодом по завершении программы (50 пульсов по 100 мс)

**slave_blink_workmode** - плавно зажигать и гасить индикаторный светодиод в цикле

**master_init** - светодиод на плате мигает после инициализации master-контроллера. (4 пульса по 500мс)

**master_RTS_changer** - контроллер master платы после получения команды от оператора ("s") отправляет команду на все slave платы, чтобы они изменили состояние RTS на противоположное, затем мониторит фронты (восходящие, если линия была опущена и наоборот).

**master_load** - после получения стартового символа разослать по дочерним платам команды на максимальную загрузку в течении 2 мин (это может быть сообщение в шину, например "010" для конкретной прошивки, чтоб лишние платы не включились, если их вдруг забыли достать или "s")

**master_repeater** - пересылает показания датчика тока и термопар в UART терминал

**master_sensor_logger** - записывает показания датчика тока и термопар

**master_RTS_logger** - фиксирует время между поднятием первой и последней линии RTS

**master_sensor_numerator** - сравнивает показания датчиков между запусками. если в пределах 5% - PASS

**master_RTS_logger_numerator** - сравнивает временное расхождение между поднятиями\опусканиями линий RTS. если в пределах 5% - PASS

**master_blink** - слушает RS, когда видит сообщение от последней платы - мигает светодиодом и пишет в шину своё сообщение

**master_blink_pass** - сравнивает замеры первого теста с последующими, если расхождения в пределах нормы - сигнализирует индикаторным светодиодом об успешном прохождении теста (пульсы по 2 сек, на протяжении 30 сек), иначе пульсы по 5 сек.


### Прошивки

---

Сценарий 1

**master_010**:
- master_init
- master_RTS_changer

**master_011**:
- master_init
- master_RTS_changer
- master_load
- slave_blink_workmode

**master_012**:
- master_init
- master_blink

**master_013**:
- master_init
- master_blink
- master_load
- slave_blink_workmode

**slave_010**:
- slave_init
- slave_RTS_updown


блок нагрузочных прошивок для теста 011
**slave_011_triac_load**:
- slave_init
- slave_RTS_updown
- slave_triac_load
- slave_blink_workmode
- slave_blink

**slave_011_analog_load**:
- slave_init
- slave_RTS_updown
- slave_analog_load
- slave_blink_workmode
- slave_blink

**slave_011_digit_load**:
- slave_init
- slave_RTS_updown
- slave_digit_load
- slave_blink_workmode
- slave_blink

**slave_011_dc_load**:
- slave_init
- slave_RTS_updown
- slave_dc_load
- slave_blink_workmode
- slave_blink

**slave_011_step_load**:
- slave_init
- slave_RTS_updown
- slave_step_load
- slave_blink_workmode
- slave_blink

**slave_011_QR_load**:
- slave_init
- slave_RTS_updown
- slave_QR_load
- slave_blink_workmode
- slave_blink

**slave_012_triac**:
- slave_init
- slave_parad

**slave_012_analog**:
- slave_init
- slave_parad

**slave_012_digit**:
- slave_init
- slave_parad

**slave_012_dc**:
- slave_init
- slave_parad

**slave_012_step**:
- slave_init
- slave_parad

**slave_012_QR**:
- slave_init
- slave_parad


блок нагрузочных прошивок для теста 013
**slave_013_triac_load**:
- slave_init
- slave_parad
- slave_triac_load
- slave_blink

**slave_013_analog_load**:
- slave_init
- slave_parad
- slave_analog_load
- slave_blink

**slave_013_digit_load**:
- slave_init
- slave_parad
- slave_digit_load
- slave_blink

**slave_013_dc_load**:
- slave_init
- slave_parad
- slave_dc_load
- slave_blink

**slave_013_step_load**:
- slave_init
- slave_parad
- slave_step_load
- slave_blink

**slave_013_QR_load**:
- slave_init
- slave_parad
- slave_QR_load
- slave_blink

---

Сценарий 2

**master_020**:
- master_init
- master_repeater
- master_load


**slave_021_digit_load**:
- slave_init
- slave_digit_load
- slave_blink_workmode
- slave_blink

**slave_021_step_load**:
- slave_init
- slave_step_load
- slave_blink_workmode
- slave_blink

**slave_022_step_load**:
- slave_init
- slave_digit_load_max
- slave_blink_workmode
- slave_blink

**slave_022_dc_load**:
- slave_init
- slave_dc_load
- slave_blink_workmode
- slave_blink

**slave_022_QR_load**:
- slave_init
- slave_QR_load
- slave_blink
- slave_blink_workmode
- slave_blink

**slave_023_step_load**:
- slave_init
- slave_step_load_min
- slave_blink_workmode
- slave_blink

**slave_023_dc_load**:
- slave_init
- slave_blink_workmode
- slave_blink

**slave_024_triac_load**:
- slave_init
- slave_triac_load
- slave_blink_workmode
- slave_blink

**slave_024_analog_load**:
- slave_init
- slave_analog_load
- slave_analog_readout
- slave_analog_forced_readout
- slave_analog_relay_switcher
- slave_blink_workmode
- slave_blink

---

Сценарий 3

**master_030**:
- master_init
- master_repeater
- master_sensor_logger
- master_blink_pass
- master_load
- master_load
- master_load


**master_031**:
- master_init
- master_RTS_changer
- master_RTS_logger
- master_blink_pass
- master_load
- master_load
- master_load


**slave_030_triac_load**:
- slave_init
- slave_triac_load
- slave_blink_workmode
- slave_blink


**slave_030_analog_load**:
- slave_init
- slave_analog_readout
- slave_analog_forced_readout
- slave_blink_workmode
- slave_blink

**slave_030_digit_load**:
- slave_init
- slave_digit_load
- slave_blink_workmode
- slave_blink

**slave_030_dc_load**:
- slave_init
- slave_dc_load
- slave_blink_workmode
- slave_blink

**slave_030_step_load**:
- slave_init
- slave_step_load_max
- slave_blink_workmode
- slave_blink

**slave_030_QR_load**:
- slave_init
- slave_QR_load
- slave_blink_workmode
- slave_blink

---

Сценарий 4

**master_040**:
- master_init
- master_blink

**slave_040_triac**:
- slave_init
- slave_parad

**slave_040_analog**:
- slave_init
- slave_parad

**slave_040_digit**:
- slave_init
- slave_parad

**slave_040_dc**:
- slave_init
- slave_parad

**slave_040_step**:
- slave_init
- slave_parad

**slave_040_QR**:
- slave_init
- slave_parad

---

Сценарий 5

получение символов "r" и "s" и вывод соответствующих деталей или инициация выполнения нагрузочной программы

**master_050**:


Эта прошивка, получив команду "s" от хоста, отправляет сигнал для изменения состояния RTS каждые 0.1 секунду с добавлением сдвига, определяемого порядковым номером платы на материнской плате. Сдвиг рассчитывается как +0.1 секунда для первой платы, +0.2 секунды для второй и так далее
**master_050_RTS_changer**:



дочерние прошивки грузят по 30 секунд и плавно мерцают светодиодом
**slave_050_triac_load**
**slave_050_analog_load**
**slave_050_digit_load**
**slave_050_dc_load**
**slave_050_step_load**
**slave_050_QR_load**


эти прошивки меняют состояние линии RTS и дублируют состояние светодиодом
**slave_050_triac_RTS_changer**
**slave_050_analog_RTS_changer**
**slave_050_digit_RTS_changer**
**slave_050_dc_RTS_changer**
**slave_050_step_RTS_changer**
**slave_050_QR_RTS_changer**

Эта прошивка на протяжении 10 секунда разгоняет ШД с 0 до максимальной скорости. На 7 секунде начать мигать светодиодом 1 раз в секунду (7-8-9), 10 сек макс скорость, 11 сек - резкий останов. 
**slave_050_step_acc**


Эта прошивка на протяжении 10 секунда разгоняет ДПТ с 0 до максимальной скорости. На 7 секунде начать мигать светодиодом 1 раз в секунду (7-8-9), 10 сек макс скорость, 11 сек - резкий останов. 
**slave_050_dc_acc**


Эта прошивка на протяжении 10 секунд открывает симисторы от 0% до 50%. На 7 секунде начать мигать светодиодом 1 раз в секунду (7-8-9), 10 сек макс мощность, 11 сек - закрыть симисторы полностью.
**slave_050_triac**



Прошивка после получения команды на старт совершает 10 коротких пульсов индикаторным светодиодом по 200мс потом 3 пульса по 1 сек, вместе с последним пульсом подаётся питание на реле печи, через 5 сек питание снимается + сразу же после получения команды на старт - открывает симисторы, питающие вентиляторы на 50%
**slave_050_analog**


Прошивка слушает дочерний RS, после получения сообщения от всех дочерних плат - шлёт во внешний RS конкатенированную строку из сообщений, полученных по внутреннему RS
**master_050_RS_talker**

Эта группа прошивок шлёт по внутреннему RS попеременно 0\1
**slave_050_triac_RS_talker**
**slave_050_analog_RS_talker**
**slave_050_dc_RS_talker**
**slave_050_QR_RS_talker**

эта прошивка вместо попеременной отправки 0\1 шлёт состояние концевиков (0 - открыт, 1 - закрыт)
**slave_050_step_RS_talker**

эта прошивка вместо попеременной отправки 0\1 шлёт состояние фототранзисторов (0 - открыт, 1 - закрыт)
**slave_050_digit_RS_talker**

Эта прошивка на протяжении 30 сек разгоняет ДПТ до максимальной скорости + шлёт в UART текущее желаемое значение скорости вращения
**slave_051_dc_load**

Эта прошивка нагружает стенд со спиралями + шлёт состояние фототранзисторов в UART
**slave_051_digit_load**

Эта прошивка после инициализации совершит 3 пульса индикаторным светодиодом по 500мс и больше ничего не делает
**slave_051_NOTHING**

