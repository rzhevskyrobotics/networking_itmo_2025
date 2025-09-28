# STP Protocol Simulation (Python)

## Описание проекта

Данный проект реализует упрощённое поведение алгоритма **STP (Spanning Tree Protocol)** на языке Python.

STP применяется на канальном уровне (L2) для предотвращения петель в сети с избыточными соединениями между коммутаторами.

В рамках проекта реализованы:

- выбор **Root Bridge** (корневого моста) на основе Bridge ID;

- назначение ролей портам (**Root Port**, **Designated Port**, **Blocked Port**);

- определение состояний портов (**FORWARDING**, **BLOCKED**);

## Структура проекта

```bash
stp_project/
│── main.py          # точка входа, запуск модели
│── bridge.py        # классы Bridge и Port
└── stp.py           # логика работы STP
```

## Запуск

Склонируйте или скачайте проект и перейдите в директорию:

```bash
cd stp_project
```

Запустите главный файл:

```bash
python main.py
```

### Файлы проекта и их назначение

- **`bridge.py`** 
  
  Содержит классы, описывающие сетевое оборудование:
  
  - **`Bridge`** — модель коммутатора (моста).
    
    - Поля:
      
      - `priority` — приоритет моста;
      
      - `mac` — MAC-адрес (используется вместе с приоритетом для формирования Bridge ID);
      
      - `bridge_id` — уникальный идентификатор моста;
      
      - `ports` — список портов;
    
    - Методы:
      
      - `add_port(port)` — добавляет порт к мосту;
  
  - **`Port`** — модель порта моста.
    
    - Поля:
      
      - `name` — имя порта (например, p1, p2);
      
      - `cost` — стоимость пути (по умолчанию 1);
      
      - `role` — роль порта в STP (Root / Designated / Blocked);
      
      - `state` — состояние порта (FORWARDING / BLOCKED / DOWN);
      
      - `link` — ссылка на подключённый порт (для будущего расширения модели).

**`stp.py`** 

Содержит логику работы протокола STP:

- **`STP`** — основной класс для запуска алгоритма.
  
  - Поля:
    
    - `bridges` — список всех мостов в сети;
    
    - `root_bridge` — выбранный Root Bridge;
  
  - Методы:
    
    - `elect_root_bridge()` — выбор корневого моста (минимальный Bridge ID);
    
    - `assign_roles()` — назначение ролей портам и установка состояний;
    
    - `run()` — полный запуск алгоритма (выбор Root Bridge + назначение ролей);

**`main.py`** 

Точка входа в программу.

- Создаёт несколько мостов и их порты;

- Передаёт их в класс `STP`;

- Запускает алгоритм;

- Печатает результат работы (Root Bridge и состояния портов);

## Пример вывода:

```bash
serg@serg-pc:/stp_project$ python main.py
Root Bridge выбран: (24576, 'aa:bb:cc:00:00:01')

Bridge (24576, 'aa:bb:cc:00:00:01')
  <Port p1 role=Designated state=FORWARDING>
  <Port p2 role=Designated state=FORWARDING>

Bridge (32768, 'aa:bb:cc:00:00:02')
  <Port p1 role=Root state=FORWARDING>
  <Port p2 role=Blocked state=BLOCKED>

Bridge (32768, 'aa:bb:cc:00:00:03')
  <Port p1 role=Root state=FORWARDING>
  <Port p2 role=Blocked state=BLOCKED>
```
