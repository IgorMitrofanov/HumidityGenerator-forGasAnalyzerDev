Программа для управления заданием уставки генератора влажности с выгрузкой логов состояния всех вспомогательных устройств.
Запись логов возможно только при запуске приложения от имени администратора. 

Описание основных переменных:

loggs - список с логами;
humidity - влажность в %;
g_1, g_2, g_tot - расход в первом канале, во втором канале и суммарный соответственно;
temp - температура в град.цел.;
pos_1, pos_2 - положение клапана 1 и 2 соответственно;
t_discr - время дискретизации модуля Пельтье в мс;
t_work - время работы модуля Пельтье в мс;
t - время текущее;
target - уставка по влажности, %;
pos_1_ctrl_signal, pos_2_ctrl_signal - управляющий сигнал на клапана 1 и 2 соответственно;
recived_data - список принятых данных с устройства;
send_data - список отправляемых данных на устройство;
delay - задержка обмена данными и вывода в консоли
event_thr - объект event библиотеки threading, нужен для остановки демон-цикла


Описание всех функций и классов:

serial_ports() - определение COM-портов системы для подключения;

class ValveCtrlSystem() -  класс для управления клапанами;
- set_target(target) -  функция класса управления клапанами, задание уставки (target);
- ctrl_signal(current_value) -  функция класса управления клапанами, выдача управляющего сигнала на основании текущего уровня влажности (current_value) 
				возвращает управляющий сигнал на оба клапана (pos_1_ctrl_signal, pos_2_ctrl_signal);

class HumidityGenerator() - класс состояния генератора влажности;
- update(recived_data, target) -  функция класса состояния генератора влажности, обновляет состояние генератора (recived_data - принятые данные с устройства,
				  target - текующая уставка);
- get_params() - функция класса состояния генератора влажности, получает данные с устройства и возвращает recived_data;
- send_params() - функция класса состояния генератора влажности, отправляет параметры на устройство в виде списка send_data 
		  [полож клапана 1 от 0 до 16000, полож клапана 2 от 0 до 16000, время дискретизации модуля Пельтье в мс, время работы модуля Пельтье в мс];
- write_loggs() - функция класса состояния генератора влажности, записывает логи состояния всех вспомогательных устройств генератора влажности в excel 
		  в папку с исполняемым файлом, возвращает имя файла для сообщения пользователю об успешной записи файла;

daemon() - функция демонического цикла для обмена данными с устройством и инициализации управления, демон процесс убивается командой event_thr.set();

choose_com_port() - функция графического интерфейса выбора COM-порта;

error_empty_com_port() - функция графического интерфейса ошибки при выборе пустого COM-порта;

main_window() - функция графического интерфейса основного окна программы

