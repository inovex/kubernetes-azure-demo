RESOURCE_GROUP=kubernetes-demo
CLUSTER_NAME=kubernetes-demo-ms-roadshow
AGENT_SIZE=Standard_D1_v2
AGENT_COUNT=2
MASTER_COUNT=1
DNS_PREFIX=kubernetes-demo
LOCATION=westeurope

all: create_rg create_cluster create_redis

create_redis:
	az redis create --location=$(LOCATION) --name=ms-roadshow-demo-redis --resource-group=$(RESOURCE_GROUP) --sku-capacity=0 --sku-family=C --sku-name=Basic --enable-non-ssl-port
	@echo "Redis Keys:"
	az redis list-keys --name=ms-roadshow-demo-redis --resource-group=$(RESOURCE_GROUP)

create_rg:
	az group create --name=$(RESOURCE_GROUP) --location=$(LOCATION)

create_cluster: 
	az acs create --orchestrator-type=kubernetes --resource-group $(RESOURCE_GROUP) --name=$(CLUSTER_NAME) --dns-prefix=$(DNS_PREFIX) --agent-vm-size $(AGENT_SIZE) --agent-count=$(AGENT_COUNT) --master-count=$(MASTER_COUNT)
	az acs kubernetes get-credentials --resource-group=$(RESOURCE_GROUP) --name=$(CLUSTER_NAME)

deploy_todo_paas:
	kubectl apply -f paas/k8s-deployment

deploy_todo_minikube:
	minikube start
	kubectl apply -f iaas/k8s-deployment
