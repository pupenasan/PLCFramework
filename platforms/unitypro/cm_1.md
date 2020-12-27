## 2.3. Класи LVL1 (CLSID=16#100x – 16#17FF)

### 2.3.1. Загальні підходи
Рекомендується використовувати наскрізну ідентифікацію технологічних змінних (ID) в межах рівня LVL1 для спрощення символьного посилання в HMI.
### 2.3.2. Клас DIVAR: дискретна вхідна змінна процесу (CLSID=16#1010)
#### DIVAR_HMI
| Атрибут | Тип  | Біт       | Опис                                                         |
| ------- | ---- | --------- | ------------------------------------------------------------ |
| STA     | UINT | 0 VRAW    | =1 – значення дискретного сигналу з DICH                     |
|         |      | 1 VAL     | =1 – значення дискретної вхідної змінної після всіх перетворень, в режимі FRC=1 може змінюватися із-зовні |
|         |      | 2 BAD     | =1 – дані недостовірні                                       |
|         |      | 3 ALDIS   | =1 – тривога тимчасово виведена з експлуатації               |
|         |      | 4 DLNK    | =1 – якщо прив’язаний до каналу                              |
|         |      | 5 ENBL    | =1 – змінна задіяна  = (NOT PRM.7 AND  DLNK)                 |
|         |      | 6 ALM     | =1 – активна технологічна тривога                            |
|         |      | 7 VALPRV  | значення на попередньому циклі                               |
|         |      | 8         |                                                              |
|         |      | 9 ISALM   | дублює аналогічний з PRM (для зручності анімації)            |
|         |      | 10 ISWRN  | дублює аналогічний з PRM (для зручності анімації)            |
|         |      | 11 WRN    | =1 – активне технологічне попередження                       |
|         |      | 12 INBUF  | =1 – змінна в буфері                                         |
|         |      | 13 FRC    | =1 – змінна в режимі форсування                              |
|         |      | 14 SML    | =1 – змінна в режимі симуляції                               |
|         |      | 15 CMDBUF | =1 – команда завантаження в буфер                            |

#### DIVAR_STA
Використовується для зручності роботи в програмі
| Атрибут | Тип  | Опис                                                         |
| ------- | ---- | ------------------------------------------------------------ |
| VRAW    | BOOL | =1 – значення дискретного сигналу з DICH                     |
| VAL     | BOOL | =1 – значення дискретної вхідної змінної після всіх перетворень, в режимі FRC=1 може змінюватися із-зовні |
| BAD     | BOOL | =1 – дані недостовірні                                       |
| ALDIS   | BOOL | =1 – тривога тимчасово виведена з експлуатації               |
| DLNK    | BOOL | =1 – якщо прив’язаний до каналу                              |
| ENBL    | BOOL | =1 – змінна задіяна = (NOT PRM.7 AND  DLNK)                  |
| ALM     | BOOL | =1 – активна технологічна тривога і не активна BAD           |
| WRN     | BOOL | =1 – активне технологічне попередження і не активні BAD та ALM |
| INBUF   | BOOL | =1 – змінна в буфері                                         |
| FRC     | BOOL | =1 – змінна в режимі форсування                              |
| SML     | BOOL | =1 – змінна в режимі симуляції                               |

#### DIVAR_CFG
| Атрибут | Тип                | Біт       | Опис                                                         |
| ------- | ------------------ | --------- | ------------------------------------------------------------ |
| ID      | UINT               |           | Унікальний ідентифікатор                                     |
| CLSID   | UINT               |           | 16#1010                                                      |
| STA     | UINT або DIVAR_STA |           | статус                                                       |
| VAL     | INT                |           | для відображення значення при не форсуванні, використовується для введення значення з буфера при форсуванні |
| PRM     | UINT               | 0 ISALM   | =1 – задіяти як технологічну тривогу                         |
|         |                    | 1 ISWRN   | =1 – задіяти як технологічне попередження                    |
|         |                    | 2 INVERSE | =1 – інвертувати сире значення                               |
|         |                    | 3 b3      |                                                              |
|         |                    | 4 b4      |                                                              |
|         |                    | 5 NRMVAL  | значення норми                                               |
|         |                    | 6 QALENBL | =1 – задіяти тривогу недостовірності каналу                  |
|         |                    | 7 DSBL    | =1 – змінна не задіяна                                       |
|         |                    | 8 b8      |                                                              |
|         |                    | 9 b9      |                                                              |
|         |                    | 10 b10    |                                                              |
|         |                    | 11 b11    |                                                              |
|         |                    | 12 b12    |                                                              |
|         |                    | 13 b13    |                                                              |
|         |                    | 14 b14    |                                                              |
|         |                    | 15 b15    |                                                              |
| CHID    | UINT               |           | Логічний номер дискретного каналу                            |
| TFLTSP  | UINT               |           | Час фільтрації в 0.1 секундах                                |
| TDEASP  | UINT               |           | Час затримки тривоги в секундах                              |
| STEP1   | UINT               |           | Крок                                                         |
| TSTEP1  | UINT               |           | Час кроку в 0.1 с                                            |

#### Команди для буферу (див. структуру буферу)
| Атрибут | Тип  | Біт  | Опис                                                         |
| ------- | ---- | ---- | ------------------------------------------------------------ |
| CMD     | UINT |      | Команди:<br/>16#0100 – прочитати конфігурація <br/>16#0101 – записати конфігурація<br/>16#0102 – записати конфігурація за замовченням<br/>16#0200 – тимчасово вивести тривогу з обслуговування <br/>16#0201 – ввести тривогу в обслуговування <br/>16#0300 – форсувати/дефорсувати<br/>16#0301 – форсувати<br/>16#0302 – дефорсувати |

### 2.3.3. Клас DOVAR: дискретна вихідна змінна процесу (CLSID=16#1020)
MVAL – зберігає ручне значення. У режимі форсування на змінну HMI та перетворення RVAL замість VAL йде MVAL.
#### DOVAR_HMI
| Атрибут | Тип  | Біт       | Опис                                         |
| ------- | ---- | --------- | -------------------------------------------- |
| STA     | UINT | 0 VRAW    | =1 – значення дискретного сигналу з DOCH     |
|         |      | 1 VAL     | =1 – значення дискретної вихідної змінної    |
|         |      | 2 BAD     | =1 – дані недостовірні                       |
|         |      | 3         |                                              |
|         |      | 4 DLNK    | =1 – якщо прив’язаний до каналу              |
|         |      | 5 ENBL    | =1 – змінна задіяна  = (NOT PRM.7 AND  DLNK) |
|         |      | 6 b6      |                                              |
|         |      | 7 VALPRV  | значення на попередньому циклі               |
|         |      | 8 b8      |                                              |
|         |      | 9 MVAL    | =1 – ручне (форсоване) значення              |
|         |      | 10 b10    |                                              |
|         |      | 11 b11    |                                              |
|         |      | 12 INBUF  | =1 – змінна в буфері                         |
|         |      | 13 FRC    | =1 – змінна в режимі форсування              |
|         |      | 14 SML    | =1 – змінна в режимі симуляції               |
|         |      | 15 CMDBUF | =1 – команда завантаження в буфер            |

#### DOVAR_STA
Використовується для зручності роботи в програмі
| Атрибут | Тип  | Опис                                                         |
| ------- | ---- | ------------------------------------------------------------ |
| VRAW    | BOOL | =1 – значення дискретного сигналу з DOCH                     |
| VAL     | BOOL | =1 – значення дискретної вихідної змінної після всіх перетворень, в режимі FRC=1 може змінюватися із-зовні |
| BAD     | BOOL | =1 – дані недостовірні                                       |
| DLNK    | BOOL | =1 – якщо прив’язаний до каналу                              |
| ENBL    | BOOL | =1 – змінна задіяна = (NOT PRM.7 AND  DLNK)                  |
| VALPRV  | BOOL | значення на попередньому циклі                               |
| INBUF   | BOOL | =1 – змінна в буфері                                         |
| FRC     | BOOL | =1 – змінна в режимі форсування                              |
| SML     | BOOL | =1 – змінна в режимі симуляції                               |

#### DOVAR_CFG
| Атрибут | Тип                | Біт       | Опис                                                         |
| ------- | ------------------ | --------- | ------------------------------------------------------------ |
| ID      | UINT               |           | Унікальний ідентифікатор                                     |
| CLSID   | UINT               |           | 16#1020                                                      |
| STA     | UINT або DOVAR_STA |           | статус                                                       |
| VAL     | INT                |           | дублювання при не форсуванні, використовується для введення значення з буфера при форсуванні |
| PRM     | UINT               | 0 b0      |                                                              |
|         |                    | 1 b1      |                                                              |
|         |                    | 2 INVERSE | =1 – інвертувати сире значення                               |
|         |                    | 3 b3      |                                                              |
|         |                    | 4 b4      |                                                              |
|         |                    | 5 b5      |                                                              |
|         |                    | 6 QALENBL | =1 – задіяти тривогу недостовірності каналу                  |
|         |                    | 7 DSBL    | =1 – змінна не задіяна                                       |
|         |                    | 8 b8      |                                                              |
|         |                    | 9 b9      |                                                              |
|         |                    | 10 b10    |                                                              |
|         |                    | 11 b11    |                                                              |
|         |                    | 12 b12    |                                                              |
|         |                    | 13 b13    |                                                              |
|         |                    | 14 b14    |                                                              |
|         |                    | 15 b15    |                                                              |
| CHID    | UINT               |           | Логічний номер вихідного каналу                              |
| STEP1   | UINT               |           | Крок                                                         |
| TSTEP1  | UINT               |           | Час кроку в 0.1 с                                            |

#### Команди для буферу (див. структуру буферу):
| Атрибут | Тип  | Біт  | Опис                                                         |
| ------- | ---- | ---- | ------------------------------------------------------------ |
| CMD     | UINT |      | Команди:<br/>16#0100 – прочитати конфігурація <br/>16#0101 – записати конфігурація<br/>16#0102 – записати конфігурація за замовченням<br/>16#0200 – тимчасово вивести тривогу з обслуговування <br/>16#0201 – ввести тривогу в обслуговування <br/>16#0300 – форсувати/дефорсувати<br/>16#0301 – форсувати<br/>16#0302 – дефорсувати |

### 2.3.4. Клас AIVAR аналогова вхідна змінна процесу (CLSID=16#1030 – 16#103A)
#### Підкласи
16#1030 – загального призначення
16#1035 – внутрішні (розрахункові) змінні, значення з RVAL не використовується для розрахунку а задається зовнішнім алгоритмом, масштабування і фільтрація не використовується
#### Функції
-   класичні:
    -   режими форсування, імітації;
    -   робота з буфером
    -   формування загального статусу ПЛК;
    -   прив'язка до каналу
-   отримання сирого значення з вказаного в конфігурації каналу
-   масштабування
    -   лінійне (для усіх)
    -   кусочно-лінійною інтерполяцією (опція)
    -   інша залежність (опція)
-   фільтрація (як правило експоненціальним фільтром)
-   обробка тривог, з впливом на загальний стан тривог ПЛК:
    -   отримання тривог з каналу
    -   обрив каналу
    -   LOLO
    -   LO
    -   HI
    -   HIHI
    -   перевантаження
-   додаткові (опція):
    -   інтегрування
    -   робота за лічильним входом
    -   робота за перепадом
-   додаткові (опція) конфігураційні параметри
    -   SPL – технологічна нижня межа
    -   SPH – технологічна верхня межа
    -   ...
#### AIVAR_HMI
| Атрибут | Тип  | Біт       | Опис                                           |
| ------- | ---- | --------- | ---------------------------------------------- |
| STA     | INT  | 0 BRK     | =1 – активна тривога обриву (+ BAD)            |
|         |      | 1 OVRLD   | =1 – активна тривога КЗ (+ BAD)                |
|         |      | 2 BAD     | =1 – дані недостовірні                         |
|         |      | 3 ALDIS   | =1 – тривога тимчасово виведена з експлуатації |
|         |      | 4 DLNK    | =1 – якщо прив’язаний до каналу                |
|         |      | 5 ENBL    | =1 – змінна задіяна = (NOT PRM.7 AND  DLNK)    |
|         |      | 6 ALM     | =1 – активна технологічна тривога              |
|         |      | 7 LOLO    | =1 – активна тривога LOLO (+ALM)               |
|         |      | 8 LO      | =1 – активна тривога LO (+WRN)                 |
|         |      | 9 HI      | =1 – активна тривога HI (+WRN)                 |
|         |      | 10 HIHI   | =1 – активна тривога HIHI (+ALM)               |
|         |      | 11 WRN    | =1 – активне технологічне попередження         |
|         |      | 12 INBUF  | =1 – змінна в буфері                           |
|         |      | 13 FRC    | =1 – змінна в режимі форсування                |
|         |      | 14 SML    | =1 – змінна в режимі симуляції                 |
|         |      | 15 CMDBUF | =1 – команда завантаження в буфер              |
| VALPROC | INT  |           | значення у % від шкали вимірювання (0-10000)   |
| VAL     | REAL |           | масштабоване значення                          |

#### AIVAR_STA
Використовується для зручності роботи в програмі
| Атрибут | Тип  | Опис                                                         |
| ------- | ---- | ------------------------------------------------------------ |
| BRK     | BOOL | =1 – активна тривога обриву (+ BAD)                          |
| OVRLD   | BOOL | =1 – активна тривога КЗ (+ BAD)                              |
| BAD     | BOOL | =1 – дані недостовірні                                       |
| ALDIS   | BOOL | =1 – тривога тимчасово виведена з експлуатації               |
| DLNK    | BOOL | =1 – якщо прив’язаний до каналу                              |
| ENBL    | BOOL | =1 – змінна задіяна = (NOT PRM.7 AND  DLNK)                  |
| ALM     | BOOL | =1 – активна технологічна тривога і не активна BAD           |
| LOLO    | BOOL | =1 – активна тривога LOLO (+ALM)                             |
| LO      | BOOL | =1 – активна тривога LO (+WRN)                               |
| HI      | BOOL | =1 – активна тривога HI (+WRN)                               |
| HIHI    | BOOL | =1 – активна тривога HIHI (+ALM)                             |
| WRN     | BOOL | =1 – активне технологічне попередження і не активні BAD та ALM |
| INBUF   | BOOL | =1 – змінна в буфері                                         |
| FRC     | BOOL | =1 – змінна в режимі форсування                              |
| SML     | BOOL | =1 – змінна в режимі симуляції                               |

#### AIVAR_CFG
| Атрибут | Тип                | Біт        | Опис                                                         |
| ------- | ------------------ | ---------- | ------------------------------------------------------------ |
| ID      | UINT               |            | Унікальний ідентифікатор                                     |
| CLSID   | UINT               |            | 16#1030                                                      |
| STA     | UINT або AIVAR_STA |            | статус                                                       |
| VALPROC | INT                |            | значення у % від шкали вимірювання (0-10000)                 |
| PRM     | UINT               | 0 LOENBL   | =1 – задіяна тривога LO /MASK 16#1                           |
|         |                    | 1 HIENBL   | =1 – задіяна тривога HI /MASK 16#2                           |
|         |                    | 2 LOLOENBL | =1 – задіяна тривога LOLO /MASK 16#4                         |
|         |                    | 3 HIHIENBL | =1 – задіяна тривога HIHI/MASK 16#8                          |
|         |                    | 4 BRKENBL  | =1 – задіяна тривога обриву                                  |
|         |                    | 5 OVRLENBL | =1 – задіяна тривога перевантаження                          |
|         |                    | 6 QALENBL  | =1 – задіяна тривога перевірки якості даних                  |
|         |                    | 7 DSBL     | =1 – змінна не задіяна                                       |
|         |                    | 8 PWLENBL  | =1 – задіяна кусочно-лінійна інтерполяція (не використовувати разом з TOTALON) |
|         |                    | 9 TOTALON  | =1 – задіяне інтегрування (не використовувати разом з PWLENBL) |
|         |                    | 10 b10     |                                                              |
|         |                    | 11 b11     |                                                              |
|         |                    | 12 b12     |                                                              |
|         |                    | 13 b13     |                                                              |
|         |                    | 14 b14     |                                                              |
|         |                    | 15 b15     |                                                              |
| CHID    | UINT               |            | Логічний номер вхідного аналогового каналу CH                |
| LORAW   | INT                |            | Сире (немасштабоване) значення мінімуму                      |
| HIRAW   | INT                |            | Сире (немасштабоване) значення максимуму                     |
| VAL     | REAL               |            | масштабоване значення                                        |
| LOENG   | REAL               |            | Інженерне (масштабоване) значення мінімуму                   |
| HIENG   | REAL               |            | Інженерне (масштабоване) значення максимуму                  |
| KFLT    | REAL               |            | Коефіцієнт фільтрації для експоненціального фільтру (0.0-1.0, 0.0 – не фільтрувати), орієнтована дискретність 100 мс |
| LOSP    | REAL               |            | Уставка тривоги HI                                           |
| HISP    | REAL               |            | Уставка тривоги LO                                           |
| LOLOSP  | REAL               |            | Уставка тривоги LOLO                                         |
| HIHISP  | REAL               |            | Уставка тривоги HIHI                                         |
| THSP    | REAL               |            | Технологічна уставка HI                                      |
| TLSP    | REAL               |            | Технологічна уставка LO                                      |
| RVAL    | INT                |            | сире значення                                                |
| HYST    | UINT               |            | гістерезис (0.1 %)                                           |
| TDEALL  | UINT               |            | Час затримки на виникнення тривоги LL в 0.1 секундах         |
| TDEAL   | UINT               |            | Час затримки на виникнення тривоги L в 0.1 секундах (за необхідністю) |
| TDEAH   | UINT               |            | Час затримки на виникнення тривоги H в 0.1 секундах (за необхідністю) |
| TDEAHH  | UINT               |            | Час затримки на виникнення тривоги HH в 0.1 секундах (за необхідністю) |
| STEP1   | UINT               |            | крок                                                         |
| TSTEP1  | UINT               |            | Час кроку в 0.1 секундах                                     |

Додаткові параметри, що виділяються підключаються за необхідністю і передаються при виклику функції AIVAR_FN.
| Атрибут | Тип                   | Опис                                                         |
| ------- | --------------------- | ------------------------------------------------------------ |
| OPTR    | ARRAY [1..4] OF REAL  | Масив додаткових змінних REAL, призначення визначається параметрами:<br />**Приклад 1**: Налаштування кус очно лінійної інтерполяції<br />- VR[1] – масштабоване значення при 20% від діапазону <br/>- VR[2] – значення при 40% <br/>- VR[3] – значення при 60% <br/>- VR[4] – значення при 80%<br />**Приклад 2**: Використання інтегратору (використання REAL при великих значеннях буде втрачатися точність)<br />- VR[1] – неприведене до часу (сума) інтегроване значення за останні 10 секунд (або хвилину)  <br/>- VR[2] – приведене до часу інтегроване значення за плинну годину <br/>- VR[3] – приведене до часу інтегроване значення за попередню годину <br/>- VR[4] – приведене до часу інтегроване значення (обнуляється ззовні, наприклад по завершенню набору). |
| OPTD    | ARRAY [1..4] OF UDINT | Масив додаткових змінних UDINT                               |

#### Масштабування
Для лінійного масштабування використовується сире (не масштабоване) значення з RVAL, діапазон вхідного сигналу (LORAW, HIRAW), діапазон вихідного сигналу (LOENG, HIENG). Нижче показано масштабування в два підходи: отримання 1% від шкали (VAL1PROC), та лінійне перетворення у нефільтроване масштабоване значення VALNOFLT. Слід зазначити, що коректність діапазону перевіряється на початку функції.

![Приклад масштабування](media/2_7.png)

_рис.2.7. Приклад масштабування_

Практика використання каркасу показала, що у випадку відображення на HMI змінних у вигляді стовпчика, бажано використовувати відсоткове значення, а не абсолютне (інженерне). Це пов'язано з тим, що каркас передбачає можливість зміни діапазону значення, отже MIN і MAX шкали при цьому теж зміниться. Щоб не зберігати інженерні MIN і MAX в SCADA/HMI, можна прив'язатись до відсоткового значення, яке буде змінюватися за зміни діапазону шкали. Таким чином, в засобах відображення які потребують абсолютних діапазонів (зазначені технологічними показниками) будуть використовуватися абсолютні значення (VAL), а де необхідні значення відносно шкали – відсоткові (VALPROC). Наявність VALPROC в структурі HMI збільшує кількість тегів SCADA/HMI, тому його використання повинно бути виправданим.

У випадку масштабування за кусочно-лінійною інтерполяцією, можна використати масив додаткових проміжних точок VR. Це можуть бути масштабовані значення при 20%, 40%, 60%, 80% від шкали. Збільшення кількості додаткових змінних (які можуть бути використані не тільки для кусочно-лінійної інтерполяції) збільшує необхідну пам'ять, однак впливає на точність інтерполяції. Починаючи з версії каркасу 1.01 пам'ять (змінні) для кусочно-лінійної інтерполяції виділяється за необхідністю (масив OPTR).
#### Фільтрація.
Для фільтрації можна використати експоненціальний фільтр, який базується на попередньому значенні. Дискретність виклику впливає на вагу значення KFLT.
Нижче показаний приклад обробки значення VAL, за різних станів аналогової вхідної змінної: при форсуванні, при імітації і у звичайному режимі.

![Приклад фільтрації](media/2_8.png)

_рис.2.8. Приклад фільтрації_

Для статистичного фільтру можна використати пам'ять у масиві VR.

#### Обробник тривог
Перед формуванням алгоритму обробника тривог необхідно визначитися з замовником щодо автомату стану тривог. При формуванні алгоритму необхідно означити:
-   перелік станів:
    -   приклад 1: обрив, LOLO, LO, HI, HIHI, перевантаження
    -   приклад 2: аналогічний прикладу 1, з добавленням проміжних (перехідних) станів
-   наявність гістерезису (при виході з зони критичнішої тривоги в менш критичну)
-   наявність конфігураційного параметру (або параметрів) часової затримки на спрацювання
-   наявність часової затримки на вихід з більш критичної зони в менш критичну (це необхідно для фіксації в HMI виникнення тривоги)

Нижче показаний фрагмент програми обробки станів для тривоги, який активний тільки при активності обробки змінної (PRM_CHENBL). Тимчасові змінні (#tmpOVRLD, #tmpBRK, #tmpHI, #tmpHIHI, #tmpLO, #tmpLOLO) є умовами для спрацювання тривоги, однак не генерують їх. Умовою спрацювання тривоги є її активність (PRM_XXXENBL) та часова затримка. У даному прикладі використовується спрощений автомат станів, який не передбачає часову затримку на вихід з зони тривоги, замість цього використовується гістерезис (HYST).

![Приклад обробки станів для тривоги](media/2_9.png)

![Приклад обробки станів для тривоги](media/2_10.png)

_рис.2.9. Приклад обробки станів для тривоги_

#### Інтегрування
Для значення витратомірів можна використовувати інтегрування за часом. Слід розуміти, що точність інтегрування залежить як від значення так і від алгоритму та типу змінних, де зберігається інтеграл. Як варіант, можна використати масив VR ARRAY [1..4] OF REAL, в якому зберігати проміжні результати інтегрування, яке можна проводити як за алгоритмом прямокутників, так і трапецій. VR [1] може використовуватися для збереження суми значень витрат за певні проміжки часу, наприклад кожні 10 секунд або за хвилину. Це дасть змогу збільшити точність інтегрування, так як сумарне значення треба приводити до часу (ділити на час). Наприклад, якщо витрата вимірюється в м3/год, то через 10 секунд ми будемо мати:
(м3/3600с)\*10с,
а через хвилину
(м3/3600с)\*60с.

Таким чином, чим рідше ми перераховуємо інтегровану кількість речовини, тим менше буде знаменник, тим точніше буде значення загальної кількості на великих часових діапазонах. Однак великі інтервали розрахунку можуть не підходити для задач керування.
#### Команди для буферу (див структуру буферу)
| Атрибут | Тип  | Опис                                                         |
| ------- | ---- | ------------------------------------------------------------ |
| CMD     | UINT | Команди:<br/>16#0100 – прочитати конфігурація <br/>16#0101 – записати конфігурація<br/>16#0102 – записати конфігурація за замовченням<br/>16#0200 – тимчасово вивести тривогу з обслуговування <br/>16#0201 – ввести тривогу в обслуговування <br/>16#0300 – форсувати/дефорсувати<br/>16#0301 – форсувати<br/>16#0302 – дефорсувати |

### 2.3.5. Клас AOVAR: аналогова вхідна змінна процесу (CLSID=16#1040)
MVAL – зберігає ручне значення. У режимі форсування на змінну HMI та перетворення RVAL замість VAL йде MVAL.
#### AOVAR_HMI
| Атрибут | Тип  | Біт       | Опис                                           |
| ------- | ---- | --------- | ---------------------------------------------- |
| STA     | INT  | 0 b0      |                                                |
|         |      | 1 b1      |                                                |
|         |      | 2 BAD     | =1 – дані недостовірні                         |
|         |      | 3 ALDIS   | =1 – тривога тимчасово виведена з експлуатації |
|         |      | 4 DLNK    | =1 – якщо прив’язаний до каналу                |
|         |      | 5 ENBL    | =1 – змінна задіяна                            |
|         |      | 6 b6      |                                                |
|         |      | 7 b7      |                                                |
|         |      | 8 b8      |                                                |
|         |      | 9 b9      |                                                |
|         |      | 10 b10    |                                                |
|         |      | 11 b11    |                                                |
|         |      | 12 INBUF  | =1 – змінна в буфері                           |
|         |      | 13 FRC    | =1 – змінна в режимі форсування                |
|         |      | 14 SML    | =1 – змінна в режимі симуляції                 |
|         |      | 15 CMDBUF | =1 – команда завантаження в буфер              |
| VALPROC | INT  |           | значення у % від шкали вимірювання (0-10000)   |
| VAL     | REAL |           | масштабоване значення                          |

#### AOVAR_STA
Використовується для зручності роботи в програмі
| Атрибут | Тип  | Опис                                           |
| ------- | ---- | ---------------------------------------------- |
| BAD     | BOOL | =1 – дані недостовірні                         |
| ALDIS   | BOOL | =1 – тривога тимчасово виведена з експлуатації |
| DLNK    | BOOL | =1 – якщо прив’язаний до каналу                |
| ENBL    | BOOL | =1 – змінна задіяна                            |
| INBUF   | BOOL | =1 – змінна в буфері                           |
| FRC     | BOOL | =1 – змінна в режимі форсування                |
| SML     | BOOL | =1 – змінна в режимі симуляції                 |

#### AOVAR_CFG
| Атрибут | Тип                | Біт       | Опис                                                         |
| ------- | ------------------ | --------- | ------------------------------------------------------------ |
| ID      | UINT               |           | Унікальний ідентифікатор                                     |
| CLSID   | UINT               |           | 16#1040                                                      |
| STA     | UINT або AOVAR_STA |           | статус                                                       |
| RVAL    | INT                |           | сирі значення                                                |
| VAL     | REAL               |           | масштабоване значення                                        |
| PRM     | UINT               | 0 b0      |                                                              |
|         |                    | 1 b1      |                                                              |
|         |                    | 2 b2      |                                                              |
|         |                    | 3 b3      |                                                              |
|         |                    | 4 b4      |                                                              |
|         |                    | 5 b5      |                                                              |
|         |                    | 6 QALENBL | =1 – задіяти тривогу недостовірності каналу                  |
|         |                    | 7 DSBL    | =1 – змінна не задіяна                                       |
|         |                    | 8 PWLENBL | =1 – задіяна кусочно-лінійна інтерполяція (не використовувати разом з TOTALON) |
|         |                    | 9 b9      |                                                              |
|         |                    | 10 b10    |                                                              |
|         |                    | 11 b11    |                                                              |
|         |                    | 12 b12    |                                                              |
|         |                    | 13 b13    |                                                              |
|         |                    | 14 b14    |                                                              |
|         |                    | 15 b15    |                                                              |
| CHID    | UINT               |           | Логічний номер вихідного аналогового каналу CH               |
| LORAW   | INT                |           | Сире (немасштабоване) значення мінімуму                      |
| HIRAW   | INT                |           | Сире (немасштабоване) значення максимуму                     |
| LOENG   | REAL               |           | Інженерне (масштабоване) значення мінімуму                   |
| HIENG   | REAL               |           | Інженерне (масштабоване) значення максимуму                  |
| MVAL    | REAL               |           | ручне(форсоване) значення                                    |
| KFLT    | REAL               |           | Коефіцієнт фільтрації для експоненціального фільтру (0.0-1.0, 0.0 – не фільтрувати), орієнтована дискретність 100 мс |
| STEP1   | UINT               |           | крок                                                         |
| TSTEP1  | UINT               |           | Час кроку в 0.1 секундах                                     |

#### Команди для буферу (див структуру буферу)
| Атрибут | Тип  | Опис                                                         |
| ------- | ---- | ------------------------------------------------------------ |
| CMD     | UINT | Команди:<br/>16#0100 – прочитати конфігурація <br/>16#0101 – записати конфігурація<br/>16#0102 – записати конфігурація за замовченням<br/>16#0200 – тимчасово вивести тривогу з обслуговування <br/>16#0201 – ввести тривогу в обслуговування <br/>16#0300 – форсувати/дефорсувати<br/>16#0301 – форсувати<br/>16#0302 – дефорсувати |
### 2.3.6. Структура буферу VARBUF
| Атрибут    | Тип                   | Опис                                                         |
| ---------- | --------------------- | ------------------------------------------------------------ |
| ID         | UINT                  | Унікальний ідентифікатор                                     |
| CLSID      | UINT                  | 16#1030                                                      |
| STA        | UINT                  | статус + біт команди завантаження в буфер                    |
| CMD        | UINT                  |                                                              |
| VALPROC    | INT                   | значення у % від шкали вимірювання (0-10000)                 |
| TMP        | INT                   |                                                              |
| VAL        | REAL                  | масштабоване значення                                        |
| RVAL       | REAL                  | сирі значення                                                |
| PRM        | UINT                  | параметри                                                    |
| CHID       | UINT                  | Логічний номер вхідного аналогового каналу CH                |
| LORAW      | INT                   | AI/AO – Сире (немасштабоване) значення мінімуму              |
| HIRAW      | INT                   | AI/AO – Сире (немасштабоване) значення максимуму             |
| LOENG      | REAL                  | AI/AO – Інженерне (масштабоване) значення мінімуму           |
| HIENG      | REAL                  | AI/AO – Інженерне (масштабоване) значення максимуму          |
| KFLT/MVAL  | REAL                  | AI – Коефіцієнт фільтрації для експоненціального фільтру (0.0-1.0, 0.0 – не фільтрувати), орієнтована стабільно 100 мс<br />AO – значення форсування |
| LOSP       | REAL                  | AI – Уставка тривоги LO                                      |
| HISP       | REAL                  | AI – Уставка тривоги HI                                      |
| LOLOSP     | REAL                  | AI – Уставка тривоги LOLO                                    |
| HIHISP     | REAL                  | AI – Уставка тривоги HIHI                                    |
| LOSP_PRC   | INT                   | AI – Уставка тривоги LO у 0.01 % (для відображення)          |
| HISP_PRC   | INT                   | AI – Уставка тривоги HI у 0.01 % (для відображення)          |
| LOLOSP_PRC | INT                   | AI – Уставка тривоги LOLO у 0.01 % (для відображення)        |
| HIHISP_PRC | INT                   | AI – Уставка тривоги HIHI у 0.01 % (для відображення)        |
| HYST/TFLT  | UINT                  | AI – гістерезис (0.1 %)<br />DI – час фільтрації в 0.1 с     |
| TDEALL     | UINT                  | AI – Час затримки на виникнення тривоги LL в 0.1 секундах<br />DI – Час затримки на виникнення тривоги в секундах |
| TDEAL      | UINT                  | AI  Час затримки на виникнення тривоги L в 0.1 секундах      |
| TDEAH      | UINT                  | AI  Час затримки на виникнення тривоги H в 0.1 секундах      |
| TDEAHH     | UINT                  | AI  Час затримки на виникнення тривоги HH в 0.1 секундах     |
| STEP1      | UINT                  | крок                                                         |
| TSTEP1     | UINT                  | Час кроку в 0.1 секундах                                     |
| OPTR       | ARRAY [0..3] OF REAL  | Масив додаткових змінних REAL<br />Для АІ може використовуватись для зберігання додаткових значень технологічних вставок |
| OPTD       | ARRAY [0..3] OF UDINT | Масив додаткових змінних UDINT <br/>Приклад 1. Для АІ може використовуватись для відображення процентних значень уставок алармів:<br/>1. OPTD [0] LOLO 0.01 % (0-10000)<br/>2. OPTD [1] LO 0.01 % (0-10000)<br/>3. OPTD [2] HI 0.01 % (0-10000)<br/>4. OPTD [3] HIHI 0.01% (0-10000) |