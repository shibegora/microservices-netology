## Задача 1: Обеспечить разработку

Предложите решение для обеспечения процесса разработки: хранение исходного кода, непрерывная интеграция и непрерывная поставка. 
Решение может состоять из одного или нескольких программных продуктов и должно описывать способы и принципы их взаимодействия.

### **Предлагаемое решение: GitLab CI/CD в облаке + GitLab Runner на собственных серверах**

### **Как закрывает требования**:

| **Требование**                          | **GitLab CI/CD**                                                         |
|-----------------------------------------|--------------------------------------------------------------------------|
| **Облачная система**                    | GitLab SaaS (hosted) или GitLab self-hosted на облачных инстансах.       |
| **Система контроля версий Git**         | Встроенная система Git.                                                  |
| **Репозиторий на каждый сервис**        | Поддержка неограниченного числа репозиториев, с гибкими настройками.     |
| **Запуск сборки по событию**            | Автоматический триггер по push, merge, tag, schedule, и т.д.             |
| **Запуск сборки по кнопке**             | Ручной запуск через Pipeline с возможностью задавать параметры.          |
| **Настройки к каждой сборке**           | CI/CD Variables и Environment-specific settings для каждой сборки.       |
| **Шаблоны конфигураций**                | CI templates – шаблоны YAML-файлов для различных типов сборок.           |
| **Безопасное хранение секретов**        | Встроенное шифрованное хранилище CI/CD Variables.                        |
| **Несколько конфигураций для сборки**   | Различные .gitlab-ci.yml конфигурации и include для мульти-сборок.       |
| **Кастомные шаги сборки**               | Возможность задавать произвольные шаги в .gitlab-ci.yml.                 |
| **Собственные Docker-образы**           | Использование кастомных Docker-образов через image в CI/CD.              |
| **Агенты на собственных серверах**      | GitLab Runner может быть развёрнут локально или в облаке.                |
| **Параллельные сборки**                 | parallel и needs позволяют запускать несколько параллельных сборок.      |
| **Параллельные тесты**                  | Параллельное выполнение тестов через parallel jobs.                      |


### **Схема взаимодействия**:
1. **GitLab (облако)** – Хранит репозитории, управляет CI/CD пайплайнами.
2. **GitLab Runner на своих серверах** - на них всё собирается, тестится и запускается.
3. **Docker Registry (GitLab)** – Хранение Docker-образов.
4. **Secrets (GitLab)** – Хранение секретных данных.
5. **Конфигурация в .gitlab-ci.yml** – Определение шаблонов, параметров и шагов сборки.

### **Обоснование выбора GitLab CI/CD**:
GitLab - это комплексная платформа, которая умеет всё: хранит код, запускает сборки, прогоняет тесты и деплоит. Встроенный Git, мощные CI/CD пайплайны, куча вещей для кастомизации. Хранит секреты, docker-образы и поддерживает параллельные сборки. Всё уже готово, остаётся только настроить.


## Задача 2: Логи

Предложите решение для обеспечения сбора и анализа логов сервисов в микросервисной архитектуре.
Решение может состоять из одного или нескольких программных продуктов и должно описывать способы и принципы их взаимодействия.

### **Предлагаемое решение: Elastic Stack (ELK) + Filebeat**

### **Как это работает:**
- **Filebeat** на каждом хосте читает логи из stdout, логи просто пишутся в консоль.
- **Logstash** или напрямую **ElasticSearch** - Filebeat гонит логи по сети с гарантией доставки.
- **ElasticSearch** - центральное хранилище и движок поиска.
- **Kibana** - UI, где разработчики смогут фильтровать логи, сохранять запросы и делиться ими.

### **Соответствие требованиям**
- **Центральное хранилище** - ElasticSearch собирает логи со всех машин.
- **Минимум требований к сервисам** - просто пиши логи в stdout.
- **Гарантированная доставка** - Filebeat с retry и буферами.
- **Поиск и фильтрация** - ElasticSearch с мощным языком запросов.
- **UI и доступ разработчикам** - Kibana с ролями и дашбордами.
- **Ссылки на сохранённые поиски** - Kibana позволяет сохранять и шарить запросы.

### **Почему Elastic Stack:**
ELK - это проверенный временем набор инструментов, который идеально ложится под микросервисы. ElasticSearch - шустрая поисковая база, Logstash/Filebeat - надёжные инструменты для доставки логов, а Kibana - удобный UI для поиска, фильтрации и создания дашбордов.

## Задача 3: Мониторинг

Предложите решение для обеспечения сбора и анализа состояния хостов и сервисов в микросервисной архитектуре.
Решение может состоять из одного или нескольких программных продуктов и должно описывать способы и принципы их взаимодействия.

### **Предлагаемое решение: Prometheus + Grafana**

### **Почему Prometheus и Grafana:**
Prometheus - это стандарт де-факто для сбора и хранения метрик. Он легко собирает метрики с хостов и сервисов, а Grafana - удобный и красивый UI для визуализации, запросов и создания кастомных дашбордов. Плюсом ко всему я знаком с Prometheus больше, чем, например, с Zabbix. В ином случае, возможно, мой выбор был бы не столь очевидным.

### **Как это работает:**
- **Node Exporter** - собирает метрики CPU, RAM, HDD, Network с каждого хоста.
- **cAdvisor** - снимает метрики потребления ресурсов с Docker-контейнеров (CPU, RAM и т.д.).
- **Prometheus Exporters** - специфичные экспортеры для каждого сервиса, если нужно.
- **Prometheus** - центральный компонент, собирает метрики, хранит и даёт к ним доступ.
- **Grafana** - UI для запросов, графиков и дашбордов, плюс алерты.

### **Почему именно это решение?**
- **Метрики с хостов и сервисов** - Node Exporter и cAdvisor снимают всё, что нужно.
- **Гибкость** - можно добавить экспортеры под любой сервис.
- **Хранение метрик** - Prometheus с мощным и быстрым движком.
- **UI и дашборды** - Grafana с гибкими настройками, фильтрами и алертами.
