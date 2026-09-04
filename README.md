# Контейнеризация и оркестрация веб-приложения

Курсовой проект по дисциплине «Облачные технологии».

Проект демонстрирует полный путь веб-приложения от исходного кода до развёртывания в локальном Kubernetes-кластере. В качестве учебного приложения реализован сервис заметок с пользовательским интерфейсом, REST API и базой данных PostgreSQL.

## Что реализовано

- Контейнеризация frontend и backend с отдельными Dockerfile.
- Локальный запуск трёх сервисов через Docker Compose: Nginx, Node.js/Express и PostgreSQL.
- Изолированная Docker-сеть, именованный том для сохранения данных и передача конфигурации через переменные окружения.
- Развёртывание компонентов в Kubernetes-кластере Minikube.
- Kubernetes-ресурсы: Deployments, Services, ConfigMap, Secret, PersistentVolumeClaim и Ingress.
- Проверки работоспособности контейнеров: Docker HEALTHCHECK и Kubernetes readiness/liveness probes.
- Безостановочное обновление backend через rolling update и откат версии через rollback.
- Автоматическое горизонтальное масштабирование backend от 2 до 5 реплик с помощью HPA.
- Автоматическое восстановление Pod при сбое средствами Kubernetes.
- Базовые меры безопасности: запуск контейнеров от непривилегированных пользователей, запрет повышения привилегий и сканирование образов Trivy.

## Архитектура

```text
Browser
  |
  v
Nginx frontend
  |
  | /api/*
  v
Node.js / Express backend
  |
  v
PostgreSQL
```

Nginx отдаёт статический frontend и работает как reverse proxy для запросов к API. Backend реализует API заметок, инициализирует таблицу при запуске и хранит данные в PostgreSQL.

## Технологии

- Docker и Docker Compose
- Kubernetes, Minikube и kubectl
- Node.js 22 и Express
- PostgreSQL 17
- Nginx
- HTML, CSS и JavaScript
- Trivy

## Структура репозитория

```text
.
├── README.md
└── coursework
    ├── report.md                 # Полный отчёт по курсовой работе
    ├── screenshots/              # Скриншоты выполнения работы
    └── project
        ├── app/
        │   ├── backend/          # Express API
        │   └── frontend/         # Статический интерфейс и конфигурация Nginx
        ├── docker/               # Dockerfile компонентов
        ├── k8s/                  # Манифесты Kubernetes
        └── docker-compose.yml
```

## Документация

- [Полный отчёт по курсовой работе](coursework/report.md)
- [Подробные инструкции по запуску и демонстрации возможностей](coursework/project/README.md)

## Автор

Сторожев Данила Валерьевич, бИСТ-232, 3 курс.
