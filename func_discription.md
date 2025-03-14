### Функции

**waiting_signal** - сигнал оператору; неактивная фаза тестирования (пауза/ждёт команды оператора)
    LED1 мигает раз в секунду

**active_signal** - сигнал оператору; активная фаза тестирования (выполняются нагрузки/проверки)
    LED1 мигает 10 раз в секунду

**test_pass_signal** - сигнал оператору; тест пройден
    LED1 светится постоянно

**test_pass_fail** - сигнал оператору; тест провален
    LED1 делает три коротких пульса с паузой 2 секунды

**slave_init** - светодиод на плате мигает после инициализации slave-контроллера. (4 пульса по 500мс)

**slave_RTS_updown** - контроллер slave платы после инициализации ожидает команды на смену состояния линии RTS. Дублировать поднятое состояние RTS светодиодом на плате. 

**slave_parad** - дочерние по очереди каждая шлёт своё сообщение в шину. Шлёт, когда видит на шине сообщение предыдущей платы
(плата\_2 ждёт сообщение '111', увидела - шлёт '222', ждёт дальше)
управляющая тогглит диод, когда очередь проходит через неё
(т.е. если всё ок, на управляющей диод мерцает с одинаковой частотой). Для аналоговой платы и платы ШД - прикреплять к сообщению состояние концевиков\датчиков температуры\датчиков тока

**slave_triac_load** - открывает симисторы на 25\50\75\100% в цикле 30 секунд. Дважды

**slave_analog_load** - открывает симисторы на 25\50\75\100% в цикле 30 секунд. Дважды

**slave_analog_readout** - читает и отправляет раз в период времени по UART показания датчика тока и термопар

**slave_analog_forced_readout** - при получении команды "r", отправить на Master текущие значение температуры и датчика тока.

**slave_analog_relay_switcher** - раз в 10 сек переключать состояние реле печи на противоположное.

**slave_digit_load** - контроллер ожидает последовательной сработки всех фототранзисторов, после этого начинает параллельно грузить спирали стенда в течении 60 сек. По истечении 60 сек снять нагрузку со спиралей и зажечь индикаторный светодиод.

**slave_step_load** - ШД вращается в любую сторону со скоростью 500мм/с и максимальным для конкретного ШД ускорением до последовательного нажатия на все 4 концевика, потом вращается в другую сторону. Нажать на все концевики последовательно ещё раз - ШД остановится, после этого загорится светодиод.

**slave_step_load_max** - Оба ШД с максимальным ускорением и скоростью совершают рваные движения на протяжении 60 сек, при нажатии на любой из концевиков движение ставится на паузу, при отпускании - возвращается. Зажечь индикаторный светодиод.

**slave_step_load_min** - ШД вращается в любую сторону со скоростью 1 шаг\сек, после нажатия на 1 или 2 концевик - меняет направление вращения, если была хотя бы 1 сработка концевика - спустя 60 сек с момента старта - зажечь индикаторный светодиод. (отладочные GPIO дублируют сработку 3 и 4 концевиков)

**slave_dc_load** - ДПТ 19 сек вращается в одну сторону, 1 сек в другую, затем наоборот и так 3 раза(всего 60 сек). Зажечь индикаторный светодиод.

**slave_dc_load_max** - ДПТ вращается в любую сторону со скоростью 1 шаг\сек, после нажатия на 3 или 4 концевик, идущий к плате ШД - меняет направление вращения, если была хотя бы 1 сработка концевика - спустя 60 сек с момента старта - зажечь индикаторный светодиод. (отладочные GPIO ловят сработку с платы ШД)

**slave_QR_load** - Считывает QR код, поднимает RTS, отправляет содержимое на Master, после получения от мастера сообщения об успешном приёме, зажечь индикаторный светодиод.

**slave_blink** - моргнуть индикаторным светодиодом по завершении программы (50 пульсов по 100 мс)

**slave_blink_workmode** - плавно зажигать и гасить индикаторный светодиод в цикле

**slave_step_accelerate** - плавно разгоняет ШД с 0 до максимальной скорости за 10 секунд. На 7-9 секундах мигает светодиодом 1 раз в секунду (7-8-9), на 10 секунде - максимальная скорость, на 11 секунде - резкий останов.

**slave_dc_accelerate** - плавно разгоняет ДПТ с 0 до максимальной скорости за 10 секунд. На 7-9 секундах мигает светодиодом 1 раз в секунду (7-8-9), на 10 секунде - максимальная скорость, на 11 секунде - резкий останов.

**slave_triac_gradual** - плавно открывает симисторы от 0% до 50% за 10 секунд. На 7-9 секундах мигает светодиодом 1 раз в секунду (7-8-9), на 10 секунде - открытие симисторов 50%, на 11 секунде - полностью закрывает симисторы.

**slave_analog_sequence** - после получения команды на старт совершает 10 коротких пульсов индикаторным светодиодом по 200 мс, затем 3 пульса по 1 сек. Вместе с последним пульсом подаёт питание на реле печи, через 5 сек питание снимается. Сразу после получения команды на старт открывает симисторы, питающие вентиляторы, на 50%.

**slave_dc_load_gradual** - на протяжении 30 сек разгоняет ДПТ до максимальной скорости и отправляет в UART текущее желаемое значение скорости вращения.

**slave_digit_load_UART** - нагружает стенд со спиралями и отправляет состояние фототранзисторов в UART.

**slave_blink_3_pulses** - после инициализации совершает 3 пульса индикаторным светодиодом по 500 мс и больше ничего не делает.

**master_init** - светодиод на плате мигает после инициализации master-контроллера. (4 пульса по 500мс)

**master_RTS_changer** - контроллер master платы после получения команды от оператора ("s") отправляет команду на все slave платы, чтобы они спамили сменой состояние RTS на противоположное и обратно, затем мониторит фронты (восходящие, если линия была опущена и наоборот), если видит восходящий фронт - зажигает индикаторный светодиод на плате, если нисходящий фронт - гасит.

**master_start_rts_spam** - контроллер master платы отправляет "s" в дочерний USART

**master_rts_check** - master проверяет, работают ли rts линии
    проверяет, что все rts == 0
    шлёт "c" в дочерний usart
    пауза 1 мкс
    проверяет, что все rts == 1
    индикация тест пройден/провален
    
**slave_rts_spam** - быстро переключает rts
    (запускается, если пришёл символ 's')
    пауза (значение из константы; будет отличаться в прошивках каждой платы)
    в цикле переключать rts
    
**slave_rts_check** - по команде мастера переключает rts
    (запускается, если пришёл символ 'c')
    тогглит пин rts

**master_load** - после получения стартового символа разослать по дочерним платам команды на максимальную загрузку в течении 2 мин (это может быть сообщение в шину, например "010" для конкретной прошивки, чтоб лишние платы не включились, если их вдруг забыли достать или "s")

**master_repeater** - пересылает показания датчика тока и термопар в UART терминал

**master_sensor_logger** - записывает показания датчика тока и термопар

**master_RTS_logger** - фиксирует время между поднятием первой и последней линии RTS

**master_sensor_numerator** - сравнивает показания датчиков между запусками. если в пределах 3% - PASS

**master_RTS_logger_numerator** - сравнивает временное расхождение между поднятиями\опусканиями линий RTS. если в пределах 5% - PASS

**master_blink** - слушает RS, когда видит сообщение от последней платы - мигает светодиодом и пишет в шину своё сообщение

**master_blink_pass** - сравнивает замеры первого теста с последующими, если расхождения в пределах нормы - сигнализирует индикаторным светодиодом об успешном прохождении теста (пульсы по 2 сек, на протяжении 30 сек), иначе пульсы по 5 сек.

**master_RS_aggregator** - слушает внутренний RS, получает сообщения от всех Slave-плат, склеивает их, разделяя переносом строки (\n), и добавляет порядковый номер платы перед каждым сообщением. Итоговое сообщение отправляет во внешний RS или UART



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
- slave_step_load_max
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
- master_sensor_logger

**slave_040_triac**:
- slave_init
- slave_parad

**slave_040_analog**:
- slave_init
- slave_parad
- slave_analog_readout

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


**master_050**:
- master_init
- master_repeater
- master_sensor_numerator
- master_load
- master_blink_pass

**master_050_RTS_interrupt**:
- master_start_rts_spam
- master_rts_check

**master_050_RS_talker**:
- master_init
- master_RS_aggregator
- master_blink_pass

**slave_050_triac_load**:
- slave_init
- slave_triac_load
- slave_blink_workmode
- slave_blink

**slave_050_analog_load**:
- slave_init
- slave_analog_load
- slave_analog_readout
- slave_blink_workmode
- slave_blink

**slave_050_digit_load**:
- slave_init
- slave_digit_load
- slave_blink_workmode
- slave_blink

**slave_050_dc_load**:
- slave_init
- slave_dc_load
- slave_blink_workmode
- slave_blink

**slave_050_step_load**:
- slave_init
- slave_step_load
- slave_blink_workmode
- slave_blink

**slave_050_QR_load**:
- slave_init
- slave_QR_load
- slave_blink_workmode
- slave_blink

**slave_050_triac_RTS_changer**:
- slave_init
- slave_RTS_updown
- slave_triac_load
- slave_blink_workmode

**slave_050_analog_RTS_changer**:
- slave_init
- slave_RTS_updown
- slave_analog_readout
- slave_blink_workmode

**slave_050_digit_RTS_changer**:
- slave_init
- slave_RTS_updown
- slave_digit_load
- slave_blink_workmode

**slave_050_dc_RTS_changer**:
- slave_init
- slave_RTS_updown
- slave_dc_load
- slave_blink_workmode

**slave_050_step_RTS_changer**:
- slave_init
- slave_RTS_updown
- slave_step_load
- slave_blink_workmode

**slave_050_QR_RTS_changer**:
- slave_init
- slave_RTS_updown
- slave_QR_load
- slave_blink_workmode

**slave_050_triac_RTS_spam**:
- slave_rts_spam
- slave_rts_check

**slave_050_analog_RTS_spam**:
- slave_rts_spam
- slave_rts_check

**slave_050_digit_RTS_spam**:
- slave_rts_spam
- slave_rts_check

**slave_050_dc_RTS_spam**:
- slave_rts_spam
- slave_rts_check

**slave_050_step_RTS_spam**:
- slave_rts_spam
- slave_rts_check

**slave_050_QR_RTS_spam**:
- slave_rts_spam
- slave_rts_check

**slave_050_step_acc**:
- slave_init
- **slave_step_accelerate**
- slave_blink_workmode
- slave_blink

**slave_050_dc_acc**:
- slave_init
- **slave_dc_accelerate**
- slave_blink_workmode
- slave_blink

**slave_050_triac**:
- slave_init
- **slave_triac_gradual**
- slave_blink_workmode
- slave_blink

**slave_050_analog**:
- slave_init
- **slave_analog_sequence**
- slave_blink_workmode
- slave_blink

**slave_050_triac_RS_talker**:
- slave_init
- slave_parad
- slave_blink_workmode

**slave_050_analog_RS_talker**:
- slave_init
- slave_parad
- slave_blink_workmode

**slave_050_dc_RS_talker**:
- slave_init
- slave_parad
- slave_blink_workmode

**slave_050_QR_RS_talker**:
- slave_init
- slave_parad
- slave_blink_workmode

**slave_050_step_RS_talker**:
- slave_init
- slave_parad
- slave_blink_workmode

**slave_050_digit_RS_talker**:
- slave_init
- slave_parad
- slave_blink_workmode

**slave_051_dc_load**:
- slave_init
- slave_dc_load_gradual
- slave_blink_workmode
- slave_blink

**slave_051_digit_load**:
- slave_init
- slave_digit_load_UART
- slave_blink_workmode
- slave_blink

**slave_051_NOTHING**:
- slave_init
- slave_blink_3_pulses