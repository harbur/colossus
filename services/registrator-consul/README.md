# Registrator on CoreOS

This is a fleet service file for running [Registrator](https://github.com/progrium/registrator) on CoreOS.

## Requirements

This runs Registrator in a Consul-backed configuration.

* [harbur/consul](https://cloud.harbur.io/unitfiles/harbur/consul)

## Usage

```
fleetctl submit registrator.service
fleetctl start registrator
```

## Reference

* Based on [cap10morgan/registrator-coreos](https://github.com/cap10morgan/registrator-coreos)