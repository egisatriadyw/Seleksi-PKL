### Dokumentasi Setup MLflow dengan Docker
Dokumentasi ini memberikan instruksi untuk menyiapkan lingkungan MLflow yang di-Dockerize dengan MinIO sebagai penyimpanan objek dan PostgreSQL sebagai Database.

#### Dockerfile (`Dockerfile`)

```dockerfile
FROM python:3.10-slim-buster

# Install dependensi sistem
RUN apt-get update \
    && apt-get -y install libpq-dev gcc

# install paket Python
COPY requirements.txt /tmp
RUN pip install -r /tmp/requirements.txt
```

#### File Requirements (`requirements.txt`)

```plaintext
cryptography==42.0.5
boto3==1.34.51
mlflow==2.10.2
psycopg2==2.9.9
```

#### Variabel Lingkungan (`.env`)

```plaintext
AWS_ACCESS_KEY_ID=minio
AWS_SECRET_ACCESS_KEY=minio123
POSTGRES_USER=my_user
POSTGRES_PASSWORD=my_password
POSTGRES_DB=my_db
```

#### Docker Compose (`docker-compose.yml`)

```yaml
version: '3.7'

services:
    minio:
        restart: always
        image: minio/minio@sha256:b36dbf66046daa79a1cdc6b1a35e0e7724e3b12ee21d528c8952c30b7984c1bb
        container_name: mlflow_s3
        ports:
            - "9000:9000"
            - "9001:9001"
        command: server /data --console-address ':9001' --address ':9000'
        environment:
            - MINIO_ROOT_USER=${AWS_ACCESS_KEY_ID}
            - MINIO_ROOT_PASSWORD=${AWS_SECRET_ACCESS_KEY}
        volumes:
            - minio_data:/data

    mc:
        image: minio/mc@sha256:5ec8c7ed0aa3cde249d29d33e3790069581d9fb845a8ee2574662b7de8029abd
        depends_on:
            - minio
        container_name: mc
        env_file:
            - .env
        entrypoint: >
            /bin/sh -c "
            /tmp/wait-for-it.sh minio:9000 &&
            /usr/bin/mc alias set minio http://minio:9000 ${AWS_ACCESS_KEY_ID} ${AWS_SECRET_ACCESS_KEY} &&
            /usr/bin/mc mb minio/mlflow;
            exit 0;
            "
        volumes:
            - ./wait-for-it.sh:/tmp/wait-for-it.sh

    db:
        restart: always
        image: postgres:16.2-alpine3.19
        container_name: mlflow_db
        ports:
            - "5432:5432"
        environment:
            POSTGRES_USER: my_user
            POSTGRES_PASSWORD: my_password
            POSTGRES_DB: my_db
        volumes:
            - dbdata:/var/lib/mysql
        healthcheck:
            test: [ "CMD", "pg_isready", "-U", "my_user", "-d", "my_db" ]
            interval: 5s
            timeout: 5s
            retries: 20

    web:
        restart: always
        build: ./mlflow
        image: mlflow_server
        container_name: mlflow_server
        depends_on:
            - mc
            - db
        ports:
            - "5000:5000"
        environment:
            - MLFLOW_S3_ENDPOINT_URL=http://minio:9000
            - AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
            - AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
        command: mlflow server --backend-store-uri postgresql://my_user:my_password@db:5432/my_db --default-artifact-root s3://mlflow/ --host 0.0.0.0

volumes:
    dbdata:
    minio_data:
```

#### Instruksi Penggunaan

1. **Clone Project**: Melakukan download file ke folder tujuan di perangkat.
   ```
   git clone https://github.com/egisatriadyw/Seleksi-PKL.git
   ```

2. **Change Directory**: Change direktori ke dalam folder C-Project

   ```
   $ cd C-Project
   ```
  
4. **Memulai Layanan**: Mulai layanan Docker menggunakan Docker Compose:
   ```
   docker-compose up -d --build
   ``` 
5. **Akses UI MLflow**: UI MLflow akan dapat diakses di `http://localhost:5000`.
6. **Akses UI MinIO**:  UI MinIO dapat di akses di `http://localhost:9000`.

#### Catatan

- Pastikan Docker dan Docker Compose terinstal dan dikonfigurasi dengan benar di sistem komputer.
