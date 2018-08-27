Apache PredictionIO Docker
==========================

## Development

### Build PredictionIO Image

```
docker build -t predictionio/pio pio
```

## Usage

### Run PredictionIO with Selectable docker-compose Files

You can choose storages for event/meta/model to select docker-compose.yml.

```
docker-compose -f docker-compose.yml -f ... up
```

If you run PredictionIO with Postgresql, run as below:

```
docker-compose -f docker-compose.yml \
  -f pgsql/docker-compose.base.yml \
  -f pgsql/docker-compose.meta.yml \
  -f pgsql/docker-compose.event.yml \
  -f pgsql/docker-compose.model.yml \
  up
```

To use localfs as model storage, change as below: 

```
docker-compose -f docker-compose.yml \
  -f pgsql/docker-compose.base.yml \
  -f pgsql/docker-compose.meta.yml \
  -f pgsql/docker-compose.event.yml \
  -f localfs/docker-compose.model.yml \
  up
```
