FROM python:3.10-slim as builder

RUN apt-get update && apt-get install -y gcc python3-dev
WORKDIR /app
COPY pyproject.toml .
RUN pip install --user --upgrade pip && pip install --user .[test]

COPY . .
RUN pip install --user .

# Финальный этап (runtime)
FROM python:3.10-slim

WORKDIR /app
# Копируем только установленные пакеты из builder
COPY --from=builder /root/.local /root/.local
# Копируем только нужные файлы приложения (без тестов и т.д.)
COPY src/ src/

# Добавляем .local/bin в PATH
ENV PATH=/root/.local/bin:$PATH

CMD ["uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8049"]

