# Containerized Azure DNS Forwarder

> This project is inspired by the [DNS Forwarder VM](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder/). It provides a similar experience for a containerized environment. 

This project provides a containerized DNS server that forwards queries to Azure's internal DNS servers so that hostnames in the virtual network can be resolved from outside the network. This is helpful, for example, when you need to resolve Private Link enabled resources from your on-premises networks connected via Side-to-Side VPN or ExpressRoute. 

This Container can be deployed and exposed internally with Azure Kubernetes Service as well as Azure Container Instances.

## Get started
Below you find get started guides for AKS as well as ACI.

### Azure Kubernetes Service

You need make sure that all needed private Azure DNS zones are linked to the virtual network used for AKS. Without this the DNS forwarder will not be able to resolve them.

```
kubectl apply -f deploy.yaml
```

This will deploy the Azure DNS Forwarder container as Deployment with 3 replicas. It also creates an LoadBalancer services using an internal Azure Loadbalancer to expose the DNS forwarder internally. 

### Azure Container Instances

You can also run the DNS Forwarder as a serverless instance with ACI. Once again, you will need to make sure to expose ACI internally and make sure that all needed Azure private DNS zones are linked to the used virtual network.

```
az container create \
  --resource-group <your-rg> \
  --name dns-forwarder \
  --image docker.pkg.github.com/whiteducksoftware/az-dns-forwarder/az-dns-forwarder:latest \
  --cpu 1 \
  --memory 0.5 \
  --restart-policy always \
  --vnet <your-vnet> \
  --subnet <your-subnet> \
  --ip-address private \
  --location <your-location> \
  --os-type Linux \
  --port 53 \
  --protocol UDP
```

