# 1. Базовый образ
FROM python:3.9-slim

# 2. Рабочая директория
WORKDIR /app

# 3. Системные пакеты
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl vim && \
    rm -rf /var/lib/apt/lists/*

# 4. Python-зависимости
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# 5. Код приложения
COPY app.py .

# 6-7. Непривилегированный пользователь
RUN useradd -m -u 1000 appuser
USER appuser

# 8. Порт
EXPOSE 5000

# 9. Переменная окружения
ENV FLASK_ENV=production

# 10. Команда запуска
CMD ["python", "app.py"]
