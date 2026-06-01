FROM nginx:1.27-alpine


RUN adduser -D appuser


COPY index.html /usr/share/nginx/html/


COPY nginx.conf /etc/nginx/conf.d/default.conf


RUN chown -R appuser:appuser /usr/share/nginx/html \
    && chown -R appuser:appuser /etc/nginx/conf.d


USER appuser


EXPOSE 80


HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD wget --quiet --spider http://localhost:80/ || exit 1