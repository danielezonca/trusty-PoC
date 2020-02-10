# DMNEventVisualizerPoC
PoC DMN Event visualizer

Build and run the containers with 
```bash
./run-compose.yml
```
This script will build the jar artifacts on your local host and then will run `docker-compose`. 

The architecture is more or less like this 

![TrustyAIArch](https://user-images.githubusercontent.com/18282531/73201355-28b34500-4139-11ea-9561-4bf9e049bacf.png)

1) A producer will generate events (DMN Model register, DMN decisions) and send them to kafka (implementation copied from https://github.com/kostola/kogito-examples/tree/dmn-quarkus-listener-example/dmn-quarkus-listener-example)
2) A consumer (trusty service) will get the decisions, store them with elastic search, create grafana dashboards and make them available.

If you run `run-compose.sh` script, there will be many containers allocated: 
1) the producer
2) the consumer (alias trusty service)
3) grafana instance
4) prometheus instance
6) elastic search instance
7) kibana instance 
8) zookeeper instance
9) kafdrop instance

In the folder `sample-requests` there are 2 sample scripts that run sample requests to `localhost:8080` (to the producer). 

If you want to generate some random requests to the producer, you can run 
```bash
python3 data-generator/generator.py
```
It will generate 50 random requests for the loan-eligibility DMN.

After that you make some requests, you can navigate to 
1) `localhost:3000` for the grafana dashboard automatically generated at run time (depending on the rules that have been fired). 
2) `localhost:8180/decisions.html` to check out a debug UX (it will show all the events stored on elastic search and if you click on one item, that item will be fetched from the database and displayed).
3) `localhost:5601` for the kibana dashboard

