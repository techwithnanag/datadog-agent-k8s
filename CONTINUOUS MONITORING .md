- [x] **`Continuous monitoring`** \: It involves the ongoing\, real\-time observation and assessment of the security posture\, performance\, and compliance throughout the lifecycle\. It ensures that any vulnerabilities or issues are promptly identified and addressed\, enhancing the overall security and reliability of the software\.

- [x] \*\*`Metrics and Logs**:`
1. **Performance Metrics**\: It collects and analyzes performance metrics such as response times\, throughput\, and resource utilization\. 
2. **Security Logs**\: Logs generated by  applications are collected and analyzed for security purposes\. eg\.  unauthorized access attempts\, and suspicious activities are detected and responded\.
- [x] \*\*`Alerts**:`
1. **Performance Alerts**\: Alerts are triggered based on predefined thresholds for performance metrics such as CPU usage\, memory utilization\, and response times\. 


### `Datadog`
Datadog is a popular monitoring and analytics platform used for cloud\-scale applications\. It offers a wide range of features to monitor the performance of applications\, servers\, databases\, and other infrastructure components in real\-time\. 
Here are some of its key use cases\:
* **Infrastructure Monitoring**
* **Application Performance Monitoring \(APM\)**
* **Log Management**
* **Network Monitoring**
* **Security Monitoring**
* **Real\-time Alerts and Notifications**

### `AGENDA: Monitoring with datadog`
### Setup datadog account
### Install datadog agent on k8s cluster
* Enable matrics and logs 
* View matrics and logs


**Let\'s go to datadog [website](https://www.datadoghq.com/) for installation instructions** 
- [ ]  Sing up for free try account 
- [ ] Generate API KEY    
- [ ] Integration \-\-\- Agent \-\-\-\-\- Select your plaform for instructions
***
                         Platform\: Kubernetes                  
                               Method 1\: Using manifest files
                               Method 2\: Helm Charts
***<u>Helm installation instructions</u>*** 
```warp-runnable-command
kubectl create namespace datadog
kubectl create secret generic datadog-secret -n datadog --from-literal api-key=5bd202ce0a2d78903bfd54f38c76f1fe  

```
```warp-runnable-command

#!/bin/bash
######### create secret #############

#######  Adding datadog repo ###########################################
echo "adding datadog repo...."
sleep 5
helm repo add datadog https://helm.datadoghq.com
helm repo update

####### Fetching the helm chart #######################################
echo "fetching datadog helm chart...."
 # Check if the folder datadog exists
 if [ ! -d "datadog" ]; then
     # If it doesn't exist, pull and untar the helm chart
     helm pull --untar datadog/datadog
 fi
# helm fetch --untar datadog/datadog || true
#######   generate over-ride value file ###############################

#######   generate over-ride value file ###############################
cat << EOF > datadog/phase18-values.yaml
datadog:
  clusterName: minikube
  apm:
    portEnabled: true
    enabled: true
  logs:
    enabled: true
    containerCollectAll: true
  processAgent:
    processCollection: true
  kubelet:
    tlsVerify: false
  site: us5.datadoghq.com
  clusterAgent:
    replicas: 2
    createPodDisruptionBudget: true
  kubeStateMetricsEnabled: true
  kubeStateMetricsCore:
    enabled: true 
  apiKeyExistingSecret: datadog-secret

  prometheusScrape:
    enabled: true
    serviceEndpoints: true

EOF
###### installing helm chart #########################




sleep 5
echo "installing helm chart...."

helm upgrade -i revive-phase18 -f datadog/phase18-values.yaml \
-n datadog datadog 


```
### check if the pods are running
***kubectl get po \-n datadog***
***kubectl exec  \-it  \[ pod\-name\] \-\- agent status***
***


























































































































