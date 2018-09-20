# prestowatcher
A very simple Slack alerter for [PrestoDB](https://prestodb.io/) queries that are 
potentially unintentionally resource intensive.

## Criteria
Currently prestowatcher only supports alerting if a given source used in a query is scanning over more
than a configurable amount of partitions.

## Future
Future features might include checking for missing filters, whitelisting of queries, and more.

## Building
```
git clone https://github.com/thecubed/prestowatcher
docker build prestowatcher
```

## Running
```
Usage:
  prestowatcher [OPTIONS]

Application Options:
  -v, --verbose   Enable DEBUG logging
  -V, --version   Print version and exit
  -u, --url=      presto URL (including scheme and port) [$PRESTO_URL]
  -m, --maxpart=  Alert when Presto queries scan more than X partitions (default: 30) [$MAX_PARTITIONS]
  -i, --interval= Update interval in seconds (default: 20) [$UPDATE_INTERVAL]
  -t, --token=    Slack Webhook URL [$SLACK_URL]
  -p, --port=     Health check HTTP server port (default: 8080) [$PORT]

Help Options:
  -h, --help      Show this help message
```
Any options with a `$NAME` in the help are able to be specified as environment variables to ease deployment
in cloud environments.

The application exposes a HTTP health check at `/` which will return the last successful time it was able to check
queries in Presto. Use this as a health check if running under Marathon/Kubernetes to ensure the service isn't stuck.