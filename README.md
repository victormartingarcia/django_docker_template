# Django backend development with Docker Compose

A handy template for helping to kick start a new Django backend development environment.

## Requirements

- Docker >= v1.10.3
- Docker Compose >= v1.7.1
- Python >= 3.5

## Usage

Copy your project files on `django/` folder (make sure you don't delete `Dockerfile` and `entrypoint.sh` files)

```
docker-compose build
docker-compose up

# Create a django superuser
docker-compose exec django python manage.py createsuperuser
```

## The Stack

This is my design for an all-purpose backend, with a lot of focus on _observability_.


### Backend
- [Django](https://www.djangoproject.com/): Our business logic and frontend (either web or rest api)
- [Celery](http://docs.celeryproject.org/en/latest/): Async tasks off-loading
- [Celery Beat](http://docs.celeryproject.org/en/latest/userguide/periodic-tasks.html#introduction): Task scheduler

### Database

- [PostgreSQL](https://www.postgresql.org/): Relational data storage

### Cache

- [Redis](https://redis.io/): django cache and session storage

### Middleware

- [RabbitMQ](https://www.rabbitmq.com/): Message broker used for:
  - Celery backend
  - Offload logs and metrics. All observability events are fired and forget to RabbitMQ from business logic layer. Then, some celery tasks consume those messages and process the events
  - As Streaming server for some real-time projects
  - As [MQTT server](https://www.rabbitmq.com/mqtt.html) for some iot projects

### Observability

- [Elastic Search](https://www.elastic.co/es/): Aggregate logs either from our business logic or services
- [Kibana](https://www.elastic.co/es/products/kibana): Frontend for checking logs
- [Influxdb](https://www.influxdata.com/): Timeseries database for storing metrics
- [Chronograf](https://www.influxdata.com/time-series-platform/chronograf/): Frontend for checking metrics
- [Grafana](https://grafana.com/): Real-time dashboards showing data from elasticsearch and influxdb. Used for email alerting

## Final notes

This docker-compose stack is intended for development purposes. In case you want to run this stack in production, please start adding nginx and a proper wsgi server :)

Hope you like it! Comments and suggestions are more than welcome
