FROM python:3.11

WORKDIR /app

COPY . .

RUN pip install flask redis requests mysql-connector-python

EXPOSE 3002

CMD ["python", "index.py"]