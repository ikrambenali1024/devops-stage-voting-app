FROM python:3.11-slim-bookworm
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl && \
    rm -rf /var/lib/apt/lists/*
WORKDIR /usr/local/app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
RUN useradd -m -u 1000 appuser
COPY . .
RUN chown -R appuser:appuser /usr/local/app
USER appuser
EXPOSE 80 

HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:80/health || exit 1

CMD ["gunicorn", "app:app", "-b", "0.0.0.0:80", "--workers", "4"]
