FROM mcr.microsoft.com/presidio-anonymizer:latest

ENV PIP_ROOT_USER_ACTION=ignore \
    PIP_NO_CACHE_DIR=1 \
    PYTHONUNBUFFERED=1 \
    LOG_LEVEL=INFO

# Gunicorn (Produktionsserver)
RUN python -m pip install --no-cache-dir "flask<3" "werkzeug<3" gunicorn==21.2.0

EXPOSE 3000
CMD ["gunicorn", "--chdir", "/usr/bin", "--bind", "0.0.0.0:3000", "--workers", "2", "--threads", "4", "--timeout", "120", "app:app"]
