# Fast Lane Visualizer

## Setup (mac-osx)

- Install Docker : https://docs.docker.com/docker-for-mac/install/#download-docker-for-mac

Make sure both docker and docker-compose is up and running.
```
$ docker-compose version
```

- Create log directory for tshark
```
mkdir -p /tmp/tshark/data
touch /tmp/tshark/data/logs.data
```

- Get the Repo and Test docker-compose to start Stack services
```
git clone https://github.com/CiscoDevNet/FastLaneSelfServe.git
cd FastLaneSelfServe
docker-compose up -d
```

- Install T-shark: https://www.wireshark.org/download.html
(installing wireshark should automatically install tshark)

## Run

There are 3 elements that you want to run for the demo: docker-compose, tshark and browser (visualization)

- Run Stack services
```
cd FastLaneSelfServe //omit if pwd is already /FastLaneSelfServe
docker-compose down  //Stop previously running instances.
docker-compose up -d //Start a new one
```

**Tip: If you keep the system up for long, it might get slower because of huge amount of packet data. Restart the docker-compose once every 1-2 hour for best experience.**

- Run tshark to generate traffic

Open a new tab, and run the tshark command to start the network capture. You can optionally provide the ```ip``` option to capture from a particular IP address.

```
tshark -a duration:500 -i en0 -e frame.number -e wlan.qos -e wlan.qos.priority -e ip.src -e ip.dst -e ip.dsfield.dscp -e ip.len -Tek > /tmp/tshark/data/logs.data
```

You can apply a filter by adding ```-Y``` or ```-f``` option to the tshark command.
For example,
```
-Y "ip.src == 10.10.30.101 && ip.dst == 10.10.30.102"
```
or
```
-f "host 10.10.30.101"
```

Note: This will run tshark and do a packet capture for 500s. For the demo scenario, you can start the tshark just before every demo and then stop if after that. This will also help to keep the network dump small.

- Go to http://0.0.0.0:5000 for the real-time demo, http://0.0.0.0:5000/demo for fake-data demo

**Tip: Refresh the Page once every 1-5 mins, to clear out the cache and to bring in the real-time nature for visulizations.**

## Update (To Latest Version)

```
cd FastLaneSelfServe //omit if pwd is already /FastLaneSelfServe
docker-compose down
docker-compose pull
docker-compose up -d
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

## Concept


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
