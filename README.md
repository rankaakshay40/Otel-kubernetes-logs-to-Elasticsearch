# Otel-kubernetes-logs-to-Elasticsearch
Sending the kubernetes pod logs via filelog reciver via otel agent and sending it to Elasticsearch exporter and view it on grafana.

**Get the pods up and running for the otel collector as daemonset.**

kubectl create -f https://raw.githubusercontent.com/open-telemetry/opentelemetry-collector/v0.111.0/examples/k8s/otel-config.yaml

**Get the configmap configuration in a yaml filelike this:**

kubectl create -f https://raw.githubusercontent.com/open-telemetry/opentelemetry-collector/v0.111.0/examples/k8s/otel-config.yaml

**Make the necessary changes in the configuration and run apply command**

kubectl apply -f otel-config.yaml -n <namesapce>

Now delete one of the pods for daemonset to reflect the changes made in otel 

kubectl delete pod <pod-name>

Check the logs for the pods using this command

kubectl logs <pod-name>

Now ssh to the xmon server and see the elasticsearch and kibana are up and running as docker 

open kibana in the browser with the server-ip: 5601.

Go to index managemnet and check whether your index is created which you configured in the otel config file.
If it exist then,

1. Go to Management > Stack Management > Index Patterns.
Click Create Index Pattern.
In the Index pattern name field, enter the name of your OpenTelemetry index. If your index name is otel-logs, type otel-logs* to match all indices with that prefix.

2. Click Next Step.
3. Select the Timestamp Field
Select a timestamp field if one is available (often @timestamp or timestamp).
Click Create Index Pattern.

4. View Logs in Kibana
Go to Discover in Kibana.
Select your new index pattern from the dropdown on the left.
You should now see logs coming in from your OpenTelemetry Collector.

Now to check the logs in Grafana,

1. Create a datasource with elasticsearch
   => Give the name of the datasource
   => Url as http:// server-ip:9200
   => Under elasticsearch details , give the index name and other details
   => Click save and test
   

2.import a dashboard named Elasticsearch logs with elasticsearch as datasource from grafana labs link 

[https://grafana.com/grafana/dashboards/
](url)

Import the dashboard onto your grafana

now add the pod, namespace and other variable as needed

For pod variable: Query it as: {"find": "terms", "field": "Resource.k8s.pod.name.keyword"} This is referred from the field names in elasticsearch

for namespace variable query it as:  {"find": "terms", "field": "Resource.k8s.namespace.name.keyword"}


Important links:

[https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/exporter/elasticsearchexporter
](url)
[https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/examples/kubernetes/otel-collector.yaml
](url) 
[https://opentelemetry.io/docs/kubernetes/collector/components/#filelog-receiver
](url)
[https://grafana.com/docs/grafana-cloud/monitor-infrastructure/kubernetes-monitoring/configuration/helm-chart-config/otel-collector/](url)
