### Функции

**waiting_signal** - сигнал оператору; неактивная фаза тестирования (пауза/ждёт команды оператора)
    LED1 мигает раз в секунду

**active_signal** - сигнал оператору; активная фаза тестирования (выполняются нагрузки/проверки)
    LED1 мигает 10 раз в секунду

**test_pass_signal** - сигнал оператору; тест пройден
    LED1 светится постоянно

**test_pass_fail** - сигнал оператору; тест провален
    LED1 делает три коротких пульса с паузой 2 секунды

**slave_RTS_updown** - смена RTS по команде
    при старте прошивки rts == 0, LED2 выключен
    если в usart пришёл символ 'c', тоггл rts и LED2
        не дольше чем через 1 мкс

**slave_parad** - проверка rs
    Ждёт предыдущий символ (константа) в usart
    Шлёт свой символ (другая константа) в usart

**slave_triac_noise** - симисторы работают, создавая максимум э.м. наводок и помех
    симисторы работают парами (1, 2) и (3, 4)
        т.е. в любой момент времени работают либо 1-й и 2-й, либо 3-й и 4-й
    на старте первой работает пара (1, 2)
    в цикле
        (1, 2) работают секунду
        (3, 4) работают секунду
        (1, 2) работают секунду
        (3, 4) работают секунду
        2 секунды работают по 50%
            нечётные пп включены (1, 2)
            чётные пп включены (3, 4)

**slave_triac_real_load_1** - реалистичный сценарий работы симисторной платы (залить на 1-ю симисторную)
    пауза 30 секунд
    включает нагреватели 1 и 3 на 100% на 30 секунд

**slave_triac_real_load_2** - реалистичный сценарий работы симисторной платы (залить на 2-ю симисторную)
    включает нагреватель 1 на 50% на 60 секунд
    
**slave_triac_max_load** - максимальная нагрузка на симисторную плату
    включает нагреватели 1 и 3 на 100% на 45 секунд

**slave_analog_load** - открывает симисторы на 25\50\75\100% в цикле 30 секунд. Дважды

**slave_analog_get_temp_cur** - при получении команды "r", отправить на Master текущие значение температуры и датчика тока.

**slave_analog_relay_switcher** - раз в 10 сек переключать состояние реле печи на противоположное.

**slave_digit_load** - нагрузка выходов
    ШИМ 50% на все выходы
    через 60 секунд выключает

**slave_step_noise** - ШД движется, создавая максимум э.м. наводок и помех
    в цикле
        движения с параметрами
            speed = 600 mm/sec
            accel = 3000 mm/sec^2
        между движениями пауза 300 мс
        при каждом движении меняется направление
        расстояние выбирается случайно в промежутке 10 - 30 мм
        на 1 мм приходится 80 шагов

**slave_step_real_load** - реалистичный сценарий работы ШД
    в цикле
        движения с параметрами
            speed = 600 mm/sec
            accel = 3000 mm/sec^2
        при каждом движении меняется направление
        на 1 мм приходится 80 шагов
        делает 4 движения по 50 мм, затем 2 движения по 200 мм

**slave_step_max_24v_load** - максимальная нагрузка ШД на силовую линию
    в цикле
        движения с параметрами
            speed = 600 mm/sec
            accel = 3000 mm/sec^2
            расстояние = 20 мм
        при каждом движении меняется направление
        на 1 мм приходится 80 шагов

**slave_step_load_min** - ШД вращается в любую сторону со скоростью 1 шаг\сек, после нажатия на 1 или 2 концевик - меняет направление вращения, если была хотя бы 1 сработка концевика - спустя 60 сек с момента старта - зажечь индикаторный светодиод. (отладочные GPIO дублируют сработку 3 и 4 концевиков)

**slave_dc_noise** - ДПТ двигаются, создавая максимум э.м. наводок и помех
    оба двигателя работают на 100%
    раз в 2 секунды одновременно меняет направление обоих двигателей

**slave_dc_real_load** - реалистичный сценарий работы ДПТ
    в цикле
        оба ДПТ на 100% 19 сек вращается в одну сторону, 1 сек в другую

**slave_dc_max_24v_load** - максимальная нагрузка ДПТ на силовую линию
    оба двигателя работают на 100%
    раз в 2 секунды одновременно меняет направление обоих двигателей

**slave_QR_load** - Считывает QR код, поднимает RTS, отправляет содержимое на Master, после получения от мастера сообщения об успешном приёме, зажечь индикаторный светодиод.

**master_RTS_changer** - цикл проверки смены rts
    не раньше, чем через 2 сек после старта
    проверить, что все rts == 0
        если нет, test_pass_fail
    while true
        отправить 'c' в дочерний usart
        подождать 1 мкс
        проверить, что все rts переключились
            если нет, test_pass_fail
        в течение одной секунды
            если был пульс на любом из rts, test_pass_fail
    
контроллер master платы после получения команды от оператора ("s") отправляет команду на все slave платы, чтобы они спамили сменой состояние RTS на противоположное и обратно, затем мониторит фронты (восходящие, если линия была опущена и наоборот), если видит восходящий фронт - зажигает индикаторный светодиод на плате, если нисходящий фронт - гасит.

**master_load** - после сигнала от оператора (символ 's' в отладочном usart) отправляет команду на запуск нагрузки дочерних плат (символ 's' в дочерний usart)

**master_repeater** - пересылает показания датчика тока и термопар в UART терминал

**master_blink** - инициирует "parad" и проверяет его
    не раньше, чем через 2 сек после старта
    отправить символ 'a' в дочерний usart
    ждать символ 'l'
        если символ не приходит в течение секунды, сигнал ошибки и прекратить тест

### Прошивки

---

Сценарий 1

**master_010**:
- waiting_signal
- master_RTS_changer

**master_011**:
- waiting_signal
- master_RTS_changer
- master_load
- active_signal

**master_012**:
- waiting_signal
- master_blink

**master_013**:
- waiting_signal
- master_blink
- master_load
- active_signal

**slave_010**:
- waiting_signal
- slave_RTS_updown


блок нагрузочных прошивок для теста 011
**slave_011_triac_load**:
- waiting_signal
- slave_RTS_updown
- slave_triac_noise
- active_signal
- test_pass_signal
Комментарий:
    если пришёл 's', triac_load параллельно RTS_updown

**slave_011_analog_load**:
- waiting_signal
- slave_RTS_updown
- slave_analog_load
- active_signal
- test_pass_signal
Комментарий:
    если пришёл 's', analog_load параллельно RTS_updown

**slave_011_digit_load**:
- waiting_signal
- slave_RTS_updown
- slave_digit_load
- active_signal
- test_pass_signal
Комментарий:
    если пришёл 's', digit_load параллельно RTS_updown

**slave_011_dc_load**:
- waiting_signal
- slave_RTS_updown
- slave_dc_noise
- active_signal
- test_pass_signal
Комментарий:
    если пришёл 's', dc_load параллельно RTS_updown

**slave_011_step_load**:
- waiting_signal
- slave_RTS_updown
- slave_step_noise
- active_signal
- test_pass_signal
Комментарий:
    если пришёл 's', step_load параллельно RTS_updown

**slave_011_QR_load**:
- waiting_signal
- slave_RTS_updown
- slave_QR_load
- active_signal
- test_pass_signal
Комментарий:
    если пришёл 's', qr_load параллельно RTS_updown

**slave_012_triac**:
- waiting_signal
- slave_parad

**slave_012_analog**:
- waiting_signal
- slave_parad

**slave_012_digit**:
- waiting_signal
- slave_parad

**slave_012_dc**:
- waiting_signal
- slave_parad

**slave_012_step**:
- waiting_signal
- slave_parad

**slave_012_QR**:
- waiting_signal
- slave_parad


блок нагрузочных прошивок для теста 013
**slave_013_triac_load**:
- waiting_signal
- slave_parad
- slave_triac_noise
- test_pass_signal
Комментарий:
    если пришёл 's', triac_load параллельно parad

**slave_013_analog_load**:
- waiting_signal
- slave_parad
- slave_analog_load
- test_pass_signal
Комментарий:
    если пришёл 's', analog_load параллельно parad

**slave_013_digit_load**:
- waiting_signal
- slave_parad
- slave_digit_load
- test_pass_signal
Комментарий:
    если пришёл 's', digit_load параллельно parad

**slave_013_dc_load**:
- waiting_signal
- slave_parad
- slave_dc_noise
- test_pass_signal
Комментарий:
    если пришёл 's', dc_load параллельно parad

**slave_013_step_load**:
- waiting_signal
- slave_parad
- slave_step_noise
- test_pass_signal
Комментарий:
    если пришёл 's', step_load параллельно parad

**slave_013_QR_load**:
- waiting_signal
- slave_parad
- slave_QR_load
- test_pass_signal
Комментарий:
    если пришёл 's', qr_load параллельно parad

---

Сценарий 2

**master_020**:
- waiting_signal
- master_repeater
- master_load


**slave_021_digit_load**:
- waiting_signal
- slave_digit_load
- active_signal
- test_pass_signal

**slave_021_step_load**:
- waiting_signal
- slave_step_real_load
- active_signal
- test_pass_signal

**slave_022_dc_load**:
- waiting_signal
- slave_dc_real_load
- active_signal
- test_pass_signal

**slave_022_QR_load**:
- waiting_signal
- slave_QR_load
- test_pass_signal
- active_signal
- test_pass_signal

**slave_023_dc_load**:
- waiting_signal
- slave_dc_real_load
- active_signal
- test_pass_signal

**slave_024_1_triac_load**:
- waiting_signal
- slave_triac_real_load_1
- active_signal
- test_pass_signal

**slave_024_2_triac_load**:
- waiting_signal
- slave_triac_real_load_2
- active_signal
- test_pass_signal

**slave_024_analog_load**:
- waiting_signal
- slave_analog_load
- slave_analog_get_temp_cur
- slave_analog_relay_switcher
- active_signal
- test_pass_signal

---

Сценарий 3

**master_030**:
- waiting_signal
- master_repeater
- master_load
- master_load
- master_load


**slave_030_triac_load**:
- waiting_signal
- slave_triac_max_load
- active_signal
- test_pass_signal


**slave_030_analog_load**:
- waiting_signal
- slave_analog_get_temp_cur
- active_signal
- test_pass_signal

**slave_030_digit_load**:
- waiting_signal
- slave_digit_load
- active_signal
- test_pass_signal

**slave_030_dc_load**:
- waiting_signal
- slave_dc_max_24v_load
- active_signal
- test_pass_signal

**slave_030_step_load**:
- waiting_signal
- slave_step_max_24v_load
- active_signal
- test_pass_signal

**slave_030_QR_load**:
- waiting_signal
- slave_QR_load
- active_signal
- test_pass_signal

---

Сценарий 4

**master_040**:
- waiting_signal
- master_blink
- master_repeater

**slave_040_triac**:
- waiting_signal
- slave_parad

**slave_040_analog**:
- waiting_signal
- slave_parad
- slave_analog_get_temp_cur

**slave_040_digit**:
- waiting_signal
- slave_parad

**slave_040_dc**:
- waiting_signal
- slave_parad

**slave_040_step**:
- waiting_signal
- slave_parad

**slave_040_QR**:
- waiting_signal
- slave_parad
