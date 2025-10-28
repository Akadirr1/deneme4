FROM mcr.microsoft.com/dotnet/runtime-deps:9.0-bookworm-slim-arm64v8

WORKDIR /app

RUN apt-get update && apt-get install -y \
    curl \
    sqlite3 \
    ca-certificates \
    tzdata \
    && rm -rf /var/lib/apt/lists/*

# Non-root kullanıcı oluştur
RUN groupadd -g 1000 appuser && \
    useradd -u 1000 -g appuser -s /bin/bash -m appuser

# Self-contained publish klasöründeki dosyaları kopyala
COPY --chown=appuser:appuser . .

# Çalıştırma izni ver
RUN chmod +x /app/WebApplication5

# DB için klasör oluştur ve izinleri ayarla
RUN mkdir -p /app/data && \
    chown -R appuser:appuser /app/data && \
    chmod -R 770 /app/data

# Kullanıcı olarak çalış
USER appuser

# Healthcheck
HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
    CMD curl -f http://localhost:5000/health || exit 1

EXPOSE 5000

ENV ASPNETCORE_ENVIRONMENT=Production \
    ASPNETCORE_URLS=http://0.0.0.0:5000

ENTRYPOINT ["./WebApplication5"]
