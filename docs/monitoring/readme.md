### About:
this will guide you through setup grafana/proemtheus monitoring for your newtork using snmp


### Requirements:
docker and docker-compose installed


### Configuration:
- Prometheus Config: modify `targets` of the snmp job in `prometheus.yaml` with the addresses of your devices as well `scrape_interval` and `scrape_timeout`


### Run:
```
docker-compose up -d
```
and you can access grafana on port `3000` and prometheus on port `9090`


### Adding new devices:
- modify the `targets` in `prometheus.yaml` with your new devices
- reload prometheus config using api `curl -X POST http://localhost:9090/-/reload`
