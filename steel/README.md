# Определение температуры расплава на производстве стали.

Для оптимизации производственных расходов построена эмпирическая модель (LightGBM), предсказывающая конечную температуру стали в плавильном ковше. 
Модель учитывает не только мощность, подаваемую в расплав, но и тип и объем примесей, продув инертным газом. 
Так же произведен подробный анализ значимости признаков, позволивший упростить модель, но сохранить ее высокое качество


## Данные

Данные состоят из файлов, полученных из разных источников:

- `data_arc.csv` — данные об электродах;
- `data_bulk.csv` — данные о подаче сыпучих материалов (объём);
- `data_bulk_time.csv` *—* данные о подаче сыпучих материалов (время);
- `data_gas.csv` — данные о продувке сплава газом;
- `data_temp.csv` — результаты измерения температуры;
- `data_wire.csv` — данные о проволочных материалах (объём);
- `data_wire_time.csv` — данные о проволочных материалах (время).

## Основные выводы


* Из исходных данных сформирировали признаки
    *  __initial_temp__ - начальная температура
    *   __trw__ - время работы электродов, __trs__ - время простоя электродов,
    *  __average_rp__, __average_ap__ - средняя реактивная и активная мощность нагревающих стержней
    *  __Bulk__, __Wire__ - обьемы супучих и проволочных добавок
    *  __Gas__ - обьем продуваемого газа
    * Целевой признак - финальная температура - __final_temp__
* На этапе предобработки были удалены партии с совпадающими временами начального и конечного замера температуры, партии с аномальными значениями мощности 
* (отрицательные значения договорились считать ошибками) а так же с начальными температурами меньшими тепературами плавления стали 
* Построили несколько моделей: линейная регрессия, случайный лес, градиентный бустинг.
* Наилучший результат достигнут на LightGBM (500 деревьев, максимальная глубина 4, количество листьев 5) - __MAE = 5.5 градусов__
* Анализ значимости признаков показал, что __Bulk 8__, __Bulk 9__, __Bulk 13__ и __Wire 5__, __Wire 8__, __Wire 9__ не значимы, а реактивные и активные мощности сильно коррелированы  (corr = 0.94). После удаления незначимых добавок и реактивной мощности качество LightGBM незначительно ухудшилось - __MAE = 5.6 градусов__
## Используемые библиотеки
*pandas*, *sklearn*, *matplotlib*, *seaborn*, *lightgbm*
