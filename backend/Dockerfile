FROM python:3.13-alpine

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

RUN addgroup -S backgroup && adduser -S backuser -G backgroup
RUN chown -R backuser:backgroup /app

USER backuser

COPY main.py .

CMD ["python", "main.py"]