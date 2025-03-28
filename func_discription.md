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

**slave_analog_noise** - вентиляторы работают, создавая максимум э.м. наводок и помех
    симисторы работают на 50% (включены в чётные пп, выключены в нечётные)

**slave_analog_real_load** - реалистичный сценарий работы аналоговой платы
    открывает оба симистора на 100% на 3 минуты

**slave_analog_max_load** - максимальная нагрузка симисторной платы
    открывает оба симистора на 100% на 3 минуты

**slave_analog_get_temp_cur** - отправляет текущий ток и температуру по запросу
    если в usart пришёл 'r', отправляет температуру и ток
    ток возвращается за последний завершённый полупериод
    температура возвращается текущая
        после запроса делает 5 замеров с интервалом 10 мс, возвращает среднее
    ток замеряются на фоне
        делает не менее 10 сэмплов за полупериод
        считается как RMS

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

**slave_dc_noise** - ДПТ двигаются, создавая максимум э.м. наводок и помех
    оба двигателя работают на 100%
    раз в 2 секунды одновременно меняет направление обоих двигателей

**slave_dc_real_load** - реалистичный сценарий работы ДПТ
    в цикле
        оба ДПТ на 100% 19 сек вращается в одну сторону, 1 сек в другую

**slave_dc_max_24v_load** - максимальная нагрузка ДПТ на силовую линию
    оба двигателя работают на 100%
    раз в 2 секунды одновременно меняет направление обоих двигателей

**slave_QR_load** - настраивает usart обоих сканеров на чтение

**slave_QR_check** - читает QR с обоих сканеров, валидирует ответ
    в цикле, параллельно для обоих сканеров
        читает полученный QR код
        если он не равен QR_EXPECTED (константа), взводит rts

**master_load** - после сигнала от оператора (символ 's' в отладочном usart) отправляет команду на запуск нагрузки дочерних плат (символ 's' в дочерний usart)

**master_repeater** - пересылает показания датчика тока и термопар в UART терминал

**master_blink** - инициирует "parad" и проверяет его
    не раньше, чем через 2 сек после старта
    отправить символ 'a' в дочерний usart
    ждать символ 'l'
        если символ не приходит в течение секунды, сигнал ошибки и прекратить тест

**slave_triac_current_check** - включает симисторы для проверки тока
    ждёт последовательно сигналы на смену режима (символ 'c' в usart)
    1-й сигнал - ничего не делает
    2-й
        пауза 100 мс
        включает 1-й симистор на 10% (включён каждый 10-й полупериод)
    3-й
        пауза 100 мс
        включает 1-й симистор на 100%
    4-й
        пауза 100 мс
        включает симисторы 2,3,4 на 100%
    5-й
        пауза 100 мс
        отключает симисторы

**slave_analog_current_check** - проверяет показания датчика тока
    если во время мониторинга ток выходит за диапазон, взводит rts
    ждёт последовательно сигналы на смену режима (символ 'c' в usart)
    1-й сигнал
        мониторит, что ток <= 0.3 А
    2-й
        стоп мониторинг
        пауза 150 мс
        мониторит, что ток в диапазоне 5..6 А включая
    3-й
        стоп мониторинг
        пауза 150 мс
        мониторит, что ток в диапазоне 5..6 А включая
    4-й
        стоп мониторинг
        пауза 150 мс
        мониторит, что ток в диапазоне 15..18 А включая
    5-й
        стоп мониторинг

### Прошивки

---

Сценарий 1

**master_010**:
цикл проверки смены rts
    не раньше, чем через 2 сек после старта
    `active_signal`
    проверить, что все rts == 0
        если нет, `test_pass_fail`
    while true
        отправить 'c' в дочерний usart
        подождать 1 мкс
        проверить, что все rts переключились
            если нет, `test_pass_fail`
        в течение одной секунды
            если был пульс на любом из rts, `test_pass_fail`

**master_012**:
- master_blink
- active_signal
- test_pass_fail
Параллельно с парадом:
    Если взводится любой из rts, `test_pass_fail` и прекратить тест

**master_014**:
проверяет наводки на контуры концевиков и оптопар
(пересылает сигналы оператора, ждёт сигнал ошибок)
    пауза 2 сек
    шлёт 'c' в дочерний usart
    `active_signal`
    (далее параллельно параллельно мониторит rts)
        если в любой момент rts взвёлся, `test_pass_fail`
    ждёт 'c'
    шлёт 'c' в дочерний usart
    `waiting_signal`
    ждёт 'c'
    шлёт 'c' в дочерний usart
    `active_signal`

**master_015**:
проводит проверку датчика тока
    `active_signal`
    если до завершения теста взводится `rts_1`, ошибка
        шлёт 'e' в дочерний usart
        `tess_pass_failed`
    шлёт 'c' в дочерний usart
    4 повтора:
        пауза 10 секунд
        шлёт 'c' в дочерний usart
    `test_pass_signal`

**master_016**:
для проверки температуры
    пауза 2 сек
    на фоне:
        while true
            шлёт 'c' в дочерний usart
            считывает строку (до '\n')
            пересылает её в отладочный usart
    ждёт символ 's'
    отправляет в дочерний usart 's'
    модификация фонового потока:
        while true
            шлёт 'c' в дочерний usart
            считывает строку (до '\n')
            пересылает её в отладочный usart с префиксом "with load: "
                (т.е., если пришло "1 2\n", отправляет "with load: 1 2\n")


**slave_010**:
- slave_RTS_updown

блок нагрузочных прошивок для теста 011
**slave_011_triac_load**:
- slave_RTS_updown
- slave_triac_noise

**slave_011_analog_load**:
- slave_RTS_updown
- slave_analog_noise

**slave_011_digit_load**:
- slave_RTS_updown
- slave_digit_load

**slave_011_dc_load**:
- slave_RTS_updown
- slave_dc_noise

**slave_011_step_load**:
- slave_RTS_updown
- slave_step_noise

**slave_011_QR_load**:
- slave_RTS_updown
- slave_QR_load

**slave_012_triac**:
- slave_parad

**slave_012_analog**:
- slave_parad

**slave_012_digit**:
- slave_parad

**slave_012_dc**:
- slave_parad

**slave_012_step**:
- slave_parad

**slave_012_QR**:
- slave_parad


блок нагрузочных прошивок для теста 013
**slave_013_triac_load**:
- slave_parad
- slave_triac_noise

**slave_013_analog_load**:
- slave_parad
- slave_analog_noise

**slave_013_digit_load**:
- slave_parad
- slave_digit_load

**slave_013_dc_load**:
- slave_parad
- slave_dc_noise

**slave_013_step_load**:
- slave_parad
- slave_step_noise

**slave_013_QR_load**:
- slave_parad
- slave_QR_check

**slave_014_step_load**:
проверяет наводки на концевики
    ждёт 'c' в usart
    проверяет, что все концевики == 0
    на фоне, до получения 'c' в usart
        при любых изменениях сигнала концевиков взводит rts
    после получения 'c' в usart отключает мониторинг
    ждёт 'c' в usart
    проверяет, что все концевики == 1
    при любых изменениях сигнала концевиков взводит rts

**slave_014_di_load**:
проверяет наводки на фототранзисторы
    ждёт 'c' в usart
    включает `out_1`
    проверяет, что все оптопары == 0
    на фоне, до получения 'c' в usart
        при любых изменениях сигнала оптопар взводит rts
    после получения 'c' в usart отключает мониторинг
    ждёт 'c' в usart
    проверяет, что все оптопары == 1
    при любых изменениях сигнала оптопар взводит rts

**slave_014_triac_noise**:
- slave_triac_noise

**slave_014_analog_noise**:
- slave_analog_noise

**slave_014_dc_noise**:
- slave_dc_noise

**slave_014_QR_noise**:
- slave_QR_load

**slave_015_triac**:
- slave_triac_current_check

**slave_015_2_triac**:
Ничего не делает

**slave_015_analog**:
- slave_analog_current_check

**slave_015_2_analog**:
- slave_analog_noise

**slave_016_triac_noise**:
- slave_triac_noise
Включает шум после того, как в порт придёт 's'

**slave_016_analog**:
- slave_analog_noise
Включает шум после того, как в порт придёт 's'
Дополнительно:
    на фоне считывает температуру с обоих датчиков; 100 сэмплов в секунду
    если в порт пришло 'c', печатает температуру датчиков
        два целых числа через пробел
        температуру берёт как среднее за последние 100 сэмплов

**slave_016_2_analog**:
- slave_analog_noise
Включает шум после того, как в порт придёт 's'

**slave_016_digit_noise**:
- slave_digit_load
Включает шум после того, как в порт придёт 's'

**slave_016_dc_noise**:
- slave_dc_noise
Включает шум после того, как в порт придёт 's'

**slave_016_step_noise**:
- slave_step_noise
Включает шум после того, как в порт придёт 's'

**slave_016_QR_noise**:
- slave_QR_load

---

Сценарий 2

**master_020**:
Ничего не делает

**slave_021_digit_load**:
- slave_digit_load

**slave_021_step_load**:
- slave_step_real_load

**slave_022_dc_load**:
- slave_dc_real_load

**slave_022_QR_load**:
- slave_QR_load

**slave_024_1_triac_load**:
- slave_triac_real_load_1

**slave_024_2_triac_load**:
- slave_triac_real_load_2

**slave_024_analog_load**:
- slave_analog_real_load
- slave_analog_relay_switcher

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
- slave_analog_max_load
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
