# Multi container

Here, you will learn how to set up a Docker container running a Postgres database and connect it to a Python Flask application. The Flask application will use SQLAlchemy to interact with the database. You will explore two options: running the container using the host network and running the container using the bridge network with port mapping for Flask to access the database.

### Source code

[https://github.com/chandradeoarya/twoge](https://github.com/chandradeoarya/twoge)

## Building the application image

- Dockerfile for the application is this:

```
FROM python:alpine
ENV SQLALCHEMY_DATABASE_URI=

RUN apk update && \
    apk add --no-cache build-base libffi-dev openssl-dev
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
EXPOSE 80
CMD python app.py
```

## Creating database in postgres container

1. Creating database

   ```
   docker run -d --network host --name postgres-container \
   -e POSTGRES_USER=admin \
   -e POSTGRES_PASSWORD=dfsl324Sk0A \
   -e POSTGRES_DB=mydatabase \
   postgres:latest
   ```

2. prior to this next step run docker run -d -p 5432:5432 --name postgres-container postgres-flask Run the Docker container using the bridge network and map the Postgres container's port to a local port using the following command:

   ```
   docker run -d -p 5432:5432 --name postgres-container postgres-flask

   ```

3. In your Flask application, use the following `SQLALCHEMY_DATABASE_URI` configuration:

   ```
   SQLALCHEMY_DATABASE_URI = 'postgresql://admin:dfsl324Sk0A@localhost:5432/mydatabase'
   ```

## Using other types of network

In the above demo we just used network type host. You can try building the app with bridge network types as well.
