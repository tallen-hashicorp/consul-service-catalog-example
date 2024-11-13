# consul-service-catalog-example
Simple Service Catalog example

## To run
```bash
kubectl apply -f 0-Monitoring
kubectl apply -f 1-Standard
```

## To Access
```bash
kubectl -n consul-dc1 port-forward services/consul 8500:8500
```

## Add service via API
```bash
curl -X PUT http://127.0.0.1:8500/v1/catalog/register \
  -H "Content-Type: application/json" \
  -d '{
    "Datacenter": "dc1",
    "ID": "40e4a748-2192-161a-0510-9bf59fe950b5",
    "Node": "t2.320",
    "Address": "192.168.10.10",
    "TaggedAddresses": {
      "lan": "192.168.10.10",
      "wan": "10.0.10.10"
    },
    "NodeMeta": {
      "somekey": "somevalue"
    },
    "Service": {
      "ID": "redis1",
      "Service": "redis",
      "Tags": ["primary", "v1"],
      "Address": "127.0.0.1",
      "TaggedAddresses": {
        "lan": {
          "address": "127.0.0.1",
          "port": 8000
        },
        "wan": {
          "address": "198.18.0.1",
          "port": 80
        }
      },
      "Meta": {
        "redis_version": "4.0"
      },
      "Port": 8000
    },
    "SkipNodeUpdate": false
  }'


curl -X PUT http://127.0.0.1:8500/v1/catalog/register \
  -H "Content-Type: application/json" \
  -d '{
    "Datacenter": "dc1",
    "ID": "40e4a748-2192-161a-0510-9bf59fe950b5",
    "Node": "t2.320",
    "Address": "192.168.10.10",
    "TaggedAddresses": {
      "lan": "192.168.10.10",
      "wan": "10.0.10.10"
    },
    "NodeMeta": {
      "somekey": "somevalue"
    },
    "Service": {
      "ID": "mongodb1",
      "Service": "MongoDB",
      "Tags": ["primary", "v1"],
      "Address": "127.0.0.1",
      "TaggedAddresses": {
        "lan": {
          "address": "127.0.0.1",
          "port": 27017
        },
        "wan": {
          "address": "198.18.0.1",
          "port": 27017
        }
      },
      "Meta": {
        "mongodb_version": "4.4"
      },
      "Port": 27017
    },
    "Check": {
      "Node": "t2.320",
      "CheckID": "service:mongodb1",
      "Name": "MongoDB health check",
      "Notes": "TCP based health check",
      "Status": "passing",
      "ServiceID": "mongodb1",
      "Definition": {
        "TCP": "localhost:27017",
        "Interval": "5s",
        "Timeout": "1s"
      }
    },
    "SkipNodeUpdate": false
  }'

```

## To TearDown
```bash
kubectl delete -f 1-Standard
kubectl delete -f 0-Monitoring
```

[http://127.0.0.1:8500](http://127.0.0.1:8500)