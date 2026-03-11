# Операторно-параметрическая схема модели ремонта «Прибор-Мастер»

**Markdown с Mermaid**

```mermaid
flowchart TD
%% === БЛОК ПРИБОР ===
subgraph Прибор["Блок ПРИБОР"]
    direction TB
    h1(["ВРЕМЯ=Tслом"])==> h2["сост:=<br>сломан"]
    h2 ==> h3(["режим=<br>работа"])
    h3 ==> h4(["мастер<br>=своб"])
    h4 ==> h5["мастер:=занят<br>Tрем:=func(x)"]
    h5 ==> h6(["ВРЕМЯ=Трем"])
    h6 ==> h7["сост:=рабочий<br>мастер:=своб<br>Tслом:=func(x)"]
    h7 ==> h1
end

%% === БЛОК МАСТЕР ===
subgraph Мастер["Блок МАСТЕР"]
    direction TB
    h11["мастер:=своб<br>ожидание"] ==> h15{"мастер<br>=..."}
    h15 ==>|"...= занят"| h16["Tрем:=Трем+<br>Траб-ВРЕМЯ"]
    h16 ==> h17(["ВРЕМЯ=Траб"])
    h17 ==> h15
    h15 ==>|"...= своб"| h11
end

%% === ПАРАМЕТРЫ ===
h2 -.->par3((сост))
h7 -.->par3
par1((режим))-.->h3
par2((мастер))-.->h4
h5 -.->par2
h7 -.->par2
h5 -.->par4((Трем))
par4 -.-> h6

%% Инициатор
Ini["I::"] -.- h1
HTf["I::Tслом = 100"]

%% === СТИЛИ КЛАССОВ ===
classDef cond fill:#bee,stroke:#aaa,stroke-width:1px;
classDef state fill:#9e8,stroke:#333,stroke-width:1px;
classDef navig fill:#eda,stroke:#333,stroke-width:1px;
classDef params fill:#fcc,stroke:#159,stroke-width:1px;

class h5,h2,h7 state;
class h1,h3,h4,h6 cond;
class h15 navig;
class par1,par2,par4 params;

style par1 fill:#fcc,stroke:#111,stroke-width:2px;
style par2 fill:#fae,stroke:#bbb,stroke-width:2px;
style par4 fill:#ccc,stroke:#555,stroke-width:2px;

linkStyle 0 stroke:red,stroke-width:4px;

%% === КЛИКАБЕЛЬНЫЕ ССЫЛКИ ===
click par2 href "https://iu5.bmstu.ru" "переход для Мастера" _blank
click par1 href "https://www.bmstu.ru" "режим работы" _blank
click par4 href "https://mermaid.js.org" "время ремонта" _blank
```

## Описание схемы

- **Блок ПРИБОР**: моделирует цикл поломки и ремонта прибора. Прибор ломается (Tслом), переходит в состояние «сломан», ждёт свободного мастера, после ремонта возвращается в рабочее состояние.
- **Блок МАСТЕР**: моделирует занятость мастера. Ромб проверяет состояние «мастер занят/свободен»; при занятости выполняется ремонт (Tрем), при свободе — ожидание.
- **Параметры**: сост (состояние), режим, мастер, Трем — связывают оба блока в единую ОПС.
