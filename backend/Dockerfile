
FROM python:3.13.15-slim-trixie

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

COPY requirements.txt .

RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir -r requirements.txt \
    && groupadd --system appgroup \
    && useradd --system --gid appgroup --home-dir /app appuser

COPY --chown=appuser:appgroup app.py .

USER appuser

EXPOSE 5000

CMD ["python", "app.py"]
