FROM mcr.microsoft.com/presidio-analyzer:latest

ENV PIP_ROOT_USER_ACTION=ignore \
    PIP_NO_CACHE_DIR=1 \
    PYTHONUNBUFFERED=1 \
    LOG_LEVEL=INFO \
    NLP_ENGINE_NAME=spacy

# Runtime-Dependencies + spaCy + kleine Modelle (DE/EN)
RUN python -m pip install --no-cache-dir \
      "flask<3" "werkzeug<3" "pyyaml>=6,<7" \
      "regex>=2022.1.18" tldextract phonenumbers uritools \
      spacy==3.7.4 && \
    python -m pip install --no-cache-dir \
      https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-3.7.1/en_core_web_sm-3.7.1-py3-none-any.whl \
      https://github.com/explosion/spacy-models/releases/download/de_core_news_sm-3.7.0/de_core_news_sm-3.7.0-py3-none-any.whl

# <<< WICHTIG: NLP-Konfig überschreiben, damit KEIN en_core_web_lg autogezogen wird >>>
COPY default.yaml /usr/bin/presidio_analyzer/conf/default.yaml

# Production-WSGI-Server
RUN python -m pip install --no-cache-dir gunicorn==21.2.0


EXPOSE 3000
CMD ["gunicorn", "--chdir", "/usr/bin", "--bind", "0.0.0.0:3000", "--workers", "2", "--threads", "4", "--timeout", "120", "app:app"]
