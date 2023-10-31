# Kubernetes Community Twente Meetup 19-10-23
This is the repository for the demo given at the Kubernetes Community Twente meetup at 19 okt 2023.

It contains the code to build a simple single node Opensearch cluster with a data-prepper connected and the configuration of the OpenTelemetry collector deployed through the operator.

## Install applications

There are three part to this demo. In the first part we deploy OpenSearch and OpenTelemetry operator through an Ansible playbook. After that we make sure the data-prepper is deployed and lastly we deploy the OpenTelemetry CRDs to make sure the collector is up-and-running.

Before you execute the Ansible playbook make sure the K8S config file location is correct. In this case it is configured with K3S.

```
cd observability-demo

# Run the playbook on your local host.
ansible-playbook  ansible-playbook.yml
```

Install the Data Prepper helm chart with the following helm command. Make sure you apply to the correct namespace.
```
cd data-prepper-chart

# Do a dry run of the chart.
helm upgrade --install data-prepper . -f values.yaml --namespace opensearch --dry-run

# Do it for real.
helm upgrade --install data-prepper . -f values.yaml --namespace opensearch
```

Apply the OpenTelemetry CRDs 

```
cd opensearch-resources

# Start with the collector
kubectl apply -f opentelemetry-collector.yaml

# Next do the instrumentation.
kubectl apply -f opentelemetry-instrumentation.yaml
```

The OpenTelemetry collector should be up and running now.

## What next?

You can use the OpenTelemetry demo app to kick-start a whole environment with load generator to test the whole setup.

https://opentelemetry.io/docs/demo/
