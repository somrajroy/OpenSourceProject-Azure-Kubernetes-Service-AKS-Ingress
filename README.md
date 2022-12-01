### Demo- Azure Kubernetes Service(AKS) & Ingress  <br/>
<ins> Deploying applications in MS Azure AKS using Ingress </ins> ([NGINX Ingress Controller](https://github.com/kubernetes/ingress-nginx)) <br/>
* Microsoft Azure Kubernetes Service (AKS) is a fully managed container orchestration service. It reduces the complexity of container deployment and management and provides automation.<br/>
* Azure automatically creates and configures a Kubernetes control plane for each cluster. In addition, MS handles all Kubernetes upgrades and new version updates within AKS.<br/>
* Using AKS doesn’t attract cluster management and master node fees, unlike other platforms. Customer is charged for the network resources, worker nodes & other Azure resouces that are used.<br/>
*  Infrastructure management is smooth; AKS elastically provisions resources, so customers/developers don’t need to worry about whether they’re being used effectively.<br/>
* Azure AKS Architecture : <br/>
  A Kubernetes cluster is divided into two components: <br/>
    * Control plane: provides the core Kubernetes services and orchestration of application workloads. This is managed by Azure<br/>
    * Nodes: run client application workloads.<br/>
  ![image](https://user-images.githubusercontent.com/92582005/202123073-39cee4cb-e91e-4945-98ae-a6706cffa5cc.png) <br/>
* Ingress is a Kubernetes API object that maps an application to a stable, externally addressable HTTP or HTTPS URL. It can route traffic to multiple services<br/>
  ![image](https://user-images.githubusercontent.com/92582005/202119583-b6b598ed-8d66-44b7-9ad0-cad0c3cefcf5.png) <br/>
* Ingress controllers work at layer 7 and can use more intelligent rules to distribute application traffic. Ingress controllers typically route HTTP/HTTPS traffic to different applications based on the inbound URL.<br/>
    ![image](https://user-images.githubusercontent.com/92582005/203915170-8a64780b-2c6f-4a50-b91c-c6e04fcd1e05.png)<br/>
### Steps to be followed : <br/>  
* Clone the repository and navigate to the folder lab-05 <br/>
* Open powershell in administrator mode and login with below command. If you have multiple subscriptions then select the correct subcription by second command <br/>
  $ az login --use-device <br/>
  $ az account set --subscription <<-subsctption name/id->> (optional if you have multiple subcriptions) <br/>
* Create resource group "aksdemo". <br/>
  $ az group create --name aksdemo --location southeastasia <br/>
* Create an AKS cluster with 2 nodes (if you want to attach ACR then add --attach-acr <acrName>) <br/>
  $ az aks create --name aksdemo -g aksdemo --node-count 2 --generate-ssh-keys <br/>
* Switch context. (To configure kubectl to connect to your Kubernetes cluster, use the az aks get-credentials command). <br/>
  $ az aks get-credentials -n aksdemo -g aksdemo <br/>
* Refer below commands for verification of contexts.<br/>
  $ kubectl config view <br/>
  $ kubectl config current-context (output should be aksdemo). <br/>
  $ kubectl config get contexts <br/>
  $ kubectl config use-context <<-context name->> <br/>
* Check nodes and pods in your AKS cluster. <br/>
  $ kubectl get nodes -o wide <br/>
  $ kubectl get pods <br/>
* [Add a NGINX ingress controller](https://github.com/kubernetes/ingress-nginx) without customizing the defaults. The last command is deprecated & if 3rd command works then no need to execute last command <br/>
  $ helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx <br/>
  $ helm repo update <br/>
  $ helm install nginx-ingress ingress-nginx/ingress-nginx --create-namespace --namespace ingress-basic --set controller.replicaCount=2 --set controller.nodeSelector."kubernetes\.io/os"=linux --set defaultBackend.nodeSelector."kubernetes\.io/os"=linux <br/>
  $ helm install nginx-ingress ingress-nginx/ingress-nginx --create-namespace --namespace ingress-basic --set controller.replicaCount=2 --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux <br/>
* Verify NGINX controller installation <br/>
  $ kubectl get pods -n ingress-basic -l app.kubernetes.io/name=ingress-nginx --watch <br/>
* Inspect the ingress controller Service <br/>
  $ kubectl get svc -n ingress-basic <br/><br/>
  
  #### Deploy the application (s)
* Deploy the cats application (pod and ClusterIP service) <br/>
  $ kubectl apply -f cats.yaml <br/>
* Deploy the "Dogs" application (pod and ClusterIP service) <br/>
  $ kubectl apply -f dogs.yaml <br/>
* Deploy the "Birds" application (pod and ClusterIP service) <br/> 
  $ kubectl apply -f birds.yaml <br/>
* Create the ingress resource <br/>
  $ kubectl apply -f ingress.yaml <br/>
* List existing pods & services <br/>
  $ kubectl get pods <br/>
  $ kubectl get svc <br/>
* List existing ingress <br/>
  $ kubectl get ingress <br/>
  <br/>
#### Access the application <br/>
* Get the ingress controller External IP (type LoadBalancer) <br/>
  $ kubectl get svc -n ingress-basic <br/>
* Browse to the cats, dogs and birds service <br/>
  $ http://<<-ingress-controller-external-ip->>/cats <br/>
  $ http://<<-ingress-controller-external-ip->>/dogs <br/>
  $ http://<<-ingress-controller-external-ip->>/birds <br/>
* Clean up resources <br/>
  $ kubectl get all <br/>
  $ kubectl delete all --all <br/>
  $ kubectl delete ingress --all <br/>
  $ kubectl delete all --all -n ingress-basic <br/>
  $ kubectl delete namespace ingress-basic <br/>
* List existing resources <br/>
  $ kubectl get all <br/>
* Clean your Azure enviornment. Other than "aksdemo" there will be an autogenerated resource group created which also needs to be deleted <br/>
  $ az group delete --name <<-resource group name->> --yes -y <br/>
  $ az group delete --name aksdemo --yes -y <br/>
  <br/>
  
## Further References : <br/>
* [Kubernetes core concepts for Azure Kubernetes Service (AKS)](https://learn.microsoft.com/en-us/azure/aks/concepts-clusters-workloads) <br/>
* [Optimize compute costs on Azure Kubernetes Service (AKS)](https://learn.microsoft.com/en-us/training/modules/aks-optimize-compute-costs/)<br/>
* [Top 10 Cost Optimization Techniques for AKS Workloads running in Azure](https://www.linkedin.com/pulse/top-10-cost-optimization-techniques-aks-workloads-azure-upadhyaya)<br/>
* [Azure Kubernetes Service (AKS)](https://learn.microsoft.com/en-us/azure/aks/)<br/>
* [What is Application Gateway Ingress Controller?](https://learn.microsoft.com/en-us/azure/application-gateway/ingress-controller-overview)<br/>
* [Is ingress-nginx really a load balancer or not?](https://learn.microsoft.com/en-us/answers/questions/295210/is-ingress-nginx-is-really-a-load-balancer-or-not.html)<br/>
* [Network concepts for applications in Azure Kubernetes Service (AKS)](https://learn.microsoft.com/en-us/azure/aks/concepts-network#ingress-controllers)<br/>
  

  
  
  

