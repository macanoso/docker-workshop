# **Using docker**

First we need to run:

```bash
docker-compose up
```

to create the postgress database, and also the admin to do queries online.

Then we need to run the preprocessing pipeline. This are the steps:

1. Look up the network of the docker compose
2. Then run the following command to create the docker image: 
```bash
docker build -t taxi_ingest:v001 .
```
3. Finally run the image in this way (always is neccesary to change the network):
```bash
docker run -it \  
  --network=docker-workshop_default \ 
  taxi_ingest:v001 \
    --pg-user=root \
    --pg-pass=root \
    --pg-host=pgdatabase \
    --pg-port=5432 \
    --pg-db=ny_taxi \
    --target-table=yellow_taxi_trips_2021_2 \
    --year=2021 \
    --month=2 \
    --chunksize=100000
```

4. Then go to the http://localhost:8085/ to play with the tables


# **Using local**

First is neccesary to run the docker image of sql:


```bash
docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql \
  -p 5432:5432 \
  postgres:18
```

uv run pgcli -h localhost -p 5432 -u root -d ny_taxi


