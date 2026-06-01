# Контейнеризация и оркестрация веб-приложения

Курсовой проект по дисциплине **«Облачные технологии»**.

Тема: **«Контейнеризация и оркестрация веб-приложения: от исходного кода до развертывания в кластере»**.

Проект представляет собой многокомпонентное веб-приложение, состоящее из frontend, backend и базы данных PostgreSQL. В рамках работы были выполнены контейнеризация приложения, настройка многоконтейнерного окружения, развертывание в Kubernetes-кластере и реализация механизмов масштабирования, обновления и обеспечения безопасности.

## Структура проекта

```text
.
├── app
│   ├── backend
│   │   ├── app.js
│   │   ├── .dockerignore
│   │   ├── package.json
│   │   └── package-lock.json
│   └── frontend
│       ├── .dockerignore
│       ├── index.html
│       └── nginx.conf
├── docker
│   ├── backend
│   │   └── Dockerfile
│   └── frontend
│       └── Dockerfile
├── docker-compose.yml
├── .env
└── k8s
    ├── backend
    │   ├── configmap.yaml
    │   ├── deployment.yaml
    │   ├── hpa.yaml
    │   └── service.yaml
    ├── db
    │   ├── deployment.yaml
        ── secrets.yaml
    │   ├── pvc.yaml
    │   └── service.yaml
    ├── frontend
    │   ├── deployment.yaml
    │   └── service.yaml
    ├── ingress.yaml
```

## Используемые технологии

* Docker
* Docker Compose
* Kubernetes
* Minikube
* kubectl
* Node.js / Express
* PostgreSQL
* Nginx
* Trivy

## Требования

Для запуска проекта необходимы:

* Docker Desktop
* Docker Compose
* Minikube
* kubectl

Разработка и тестирование выполнялись в среде Windows 11.

## Переменные окружения

Создать файл `.env`:

```env
DB_NAME=notes
DB_USER=notes
DB_PASSWORD=notes
```

## Локальный запуск через Docker Compose

Сборка и запуск сервисов:

```bash
docker compose up --build -d
```

Проверка состояния контейнеров:

```bash
docker compose ps
```

Просмотр логов:

```bash
docker compose logs -f
```

Проверка работы приложения:

```bash
curl http://localhost
```

Остановка контейнеров:

```bash
docker compose down
```

Удаление контейнеров и томов:

```bash
docker compose down -v
```

## Сборка Docker-образов

Backend:

```bash
docker build -t notes-backend:1.0 -f docker/backend/Dockerfile app/backend
```

Frontend:

```bash
docker build -t notes-frontend:1.0 -f docker/frontend/Dockerfile app/frontend
```

Проверка образов:

```bash
docker images
```

Проверка истории слоев:

```bash
docker history notes-backend:1.0
```

## Развертывание в Kubernetes

Запуск Minikube:

```bash
minikube start --cpus=2 --memory=4096
```

Проверка кластера:

```bash
kubectl cluster-info
kubectl get nodes
```

Загрузка локальных образов в Minikube:

```bash
minikube image load notes-backend:1.0
minikube image load notes-frontend:1.0
```

Включение Ingress Controller:

```bash
minikube addons enable ingress
```

Применение манифестов:

```bash
kubectl apply -R -f k8s/
```

Проверка ресурсов:

```bash
kubectl get all
kubectl get ingress
kubectl get pvc
```

## Rolling Update

Сборка новой версии backend:

```bash
docker build -t notes-backend:2.0 -f docker/backend/Dockerfile app/backend
```

Загрузка новой версии:

```bash
minikube image load notes-backend:2.0
```

Обновление Deployment:

```bash
kubectl set image deployment/backend backend=notes-backend:2.0
```

Проверка процесса обновления:

```bash
kubectl rollout status deployment/backend
```

Просмотр истории:

```bash
kubectl rollout history deployment/backend
```

## Rollback

Откат к предыдущей версии:

```bash
kubectl rollout undo deployment/backend
```

Проверка результата:

```bash
kubectl rollout status deployment/backend
```

## Горизонтальное масштабирование (HPA)

Включение Metrics Server:

```bash
minikube addons enable metrics-server
```

Применение HPA:

```bash
kubectl apply -f k8s/backend/hpa.yaml
```

Проверка состояния:

```bash
kubectl get hpa
```

Наблюдение за изменением количества реплик:

```bash
kubectl get hpa -w
```

## Проверка Self-Healing

Получение списка pod:

```bash
kubectl get pods
```

Удаление одного из pod backend:

```bash
kubectl delete pod <pod-name>
```

Проверка автоматического восстановления:

```bash
kubectl get pods
```

## Проверка безопасности

Проверка пользователя внутри контейнера:

```bash
docker run --rm notes-backend:1.0 whoami
```

Сканирование образа Trivy:

```bash
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image notes-backend:1.0
```

Сканирование frontend:

```bash
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image notes-frontend:1.0
```

## Очистка ресурсов Kubernetes

Удаление всех ресурсов:

```bash
kubectl delete -R -f k8s/
```

Остановка Minikube:

```bash
minikube stop
```

Полное удаление кластера:

```bash
minikube delete
```

## Результат

В результате выполнения проекта было реализовано контейнеризированное веб-приложение, успешно развернутое в Kubernetes-кластере. Были продемонстрированы механизмы оркестрации, автоматического восстановления, горизонтального масштабирования, безопасного обновления приложений и базовые средства обеспечения безопасности контейнеров.
