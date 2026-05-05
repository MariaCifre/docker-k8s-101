FROM python:3.11-slim
WORKDIR /app
COPY reuirements.txt .
RUN pip install --no-chache-dir -r requirements.txtCOPY app ./appENV PYTHONNUNBUFFERED=1
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0"]
