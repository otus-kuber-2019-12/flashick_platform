FROM python:3.6.0-alpine
RUN addgroup -g 1001 www \
    && adduser -u 1001 -G www -s /bin/sh -D www
USER www    
WORKDIR /app
EXPOSE  8000
ENTRYPOINT ["python3", "-m", "http.server", "8000"]