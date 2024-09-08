FROM python:3.12.0a1-alpine as base

FROM base as builder
RUN mkdir /install
WORKDIR /install
COPY serviceA/requirements.txt /requirements.txt
RUN pip install "--prefix=/install" -r /requirements.txt

#Final Image
FROM base
COPY --from=builder /install /usr/local
COPY serviceA/src/A.py /app/A.py

WORKDIR /app
CMD ["gunicorn", "-w 4", "A:app","-b","0.0.0.0:8081"]
