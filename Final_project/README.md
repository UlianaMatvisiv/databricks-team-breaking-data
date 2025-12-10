# Фінальний проект для курсу "Технології обробки великих даних" 

## Тема проекту: "Дослідження трендів відеоігор через платформу Steam"

Посилання на звіт:https://docs.google.com/document/d/19k_GU2vWejJsdAXoxaftdcWbao0AJWO6eL0yNUJi5rk/edit?usp=sharing 
Посилання на презентацію:https://www.canva.com/design/DAG7AdEp4YQ/CEq1w2GM1aVPU06klMLDkA/edit?utm_content=DAG7AdEp4YQ&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton 

### Зміст
1. Початкові дані
2. ETL Pipeline
3. Використання таблиць

#### Початкові дані

Проект базується на даних, взятих з відкритих джерел (https://www.kaggle.com/datasets/fronkongames/steam-games-dataset). На інформації звідси базувалось все дослідження команди проекту, створення функціонального середовища та подальший аналіз даних з використанням технологій, які вивчались впродовж курсу.

#### ETL Pipeline

Для ефективної та професійної роботи зі знайденою командою інформацією, було прийнято рішення використати таку архітектуру:

```bash
raw data -> bronze layer -> silver layer -> gold layer
```

Таким чином, команда створила notebook (EDA.ipynb), де міститься вся кодова частина. 

**Note: для використання кодової частини необхідно запустити лише цей ноутбук, всі його частини.**

Частини коду розділені між собою та репрезентують окремі частки роботи, виконані задля подальшого аналізу всередині notebook, створення дашбордів і налаштування Genie на інтерактивну роботу з користувачем.

Таблиці, створені за допомогою EDA.ipynb, мають таку структуру:

```bash
steam_project/
├── bronze/
│   └── steam_games_raw
├── gold/
│   ├── games_free_vs_paid_share
│   ├── games_genre_trends_key_genres
│   ├── games_price_stats_by_year
│   └── games_releases_by_quarter
├── silver/
│   └── steam_games_clean
└── steam_raw_data/
    └── raw_data/
```

#### Використання таблиць

Базуючись на такій структурі, команда змогла ефективно провести дослідження цінової політики платформи Steam, тренди грових жанрів, продажі відеоігор в залежності від кварталу року, тд.


