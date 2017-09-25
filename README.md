# CLUS Fastlane Demo

## Setup (mac-osx)

- Install Docker : https://docs.docker.com/docker-for-mac/install/#download-docker-for-mac
- Install Docker-compose : https://docs.docker.com/compose/install/

- Create log directory for tshark
```
mkdir /tmp/tshark/data
touch /tmp/tshark/data/logs.data
```

- Run Stack services
```
git clone git@wwwin-github.cisco.com:DevNet/CLUS-Fastlane-Demo.git
cd CLUS-Fastlane-Demo
docker-compose up
```

- Go to 0.0.0.0:5000 for the demo


- Install T-shark: https://www.wireshark.org/download.html
(installing wireshark should automatically install tshark)

- Run tshark to generate traffic
```
tshark -a duration:500 -i en0 -e frame.number -e wlan.qos -e wlan.qos.priority -e ip.src -e ip.dst -e ip.dsfield.dscp -e ip.len -Tek > /tmp/tshark/data/logs.data
```


## Debug

- To restart, please run

```
docker-compose down
docker-compose up -d
```

- Kibana: http://127.0.0.1:5601/app/kibana (This takes a while to come up. You don't need to wait for that!)
- Elasticsearch API: http://0.0.0.0:9200
  - Index: "fastlane"
  - docType: "log"

## Comcept


- EL API to get new entries

```
//Simple Get request
GET http://0.0.0.0:9200/fastlane/log/_search?_source=json.layers.ip_dsfield_dscp,json.timestamp

//Get latest results
curl -XGET 'localhost:9200/fastlane/log/_search?_source=json.layers.ip_dsfield_dscp,json.timestamp' -H 'Content-Type: application/json' -d'
{
    "sort" : [
        {"json.timestamp.keyword" : {"order" : "desc"}}
     ]
}'

```
