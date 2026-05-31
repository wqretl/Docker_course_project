FROM nginx:1.27-alpine

# Создаем непривилегированного пользователя
RUN adduser -D appuser

# Копирование статических файлов
COPY index.html /usr/share/nginx/html/

# Копирование конфигурации nginx
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Права (важно для nginx, чтобы не было проблем)
RUN chown -R appuser:appuser /usr/share/nginx/html \
    && chown -R appuser:appuser /etc/nginx/conf.d

# Переключаемся на непривилегированного пользователя
USER appuser

# Открытие HTTP-порта
EXPOSE 80

# Проверка доступности nginx
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD wget --quiet --spider http://localhost:80/ || exit 1