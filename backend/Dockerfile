FROM python:3.12-slim
# Użytkownik systemowy bez hasła i bez katalogu domowego - na nim działa aplikacja
# Łączy obraz w GHCR z repozytorium projektu (uprawnienia pakietu dziedziczone z repozytorium)
LABEL org.opencontainers.image.source="https://github.com/dawidkro/breach-checker"
RUN useradd --system --no-create-home appuser
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
# Kod należy do root - appuser może go czytać, ale nie może go zmienić
COPY app.py .
USER appuser
EXPOSE 5000
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--workers", "2", "--no-control-socket", "app:app"]