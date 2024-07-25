# xc-appStack-maintPage
Code to build AppStack to host and display a maintenance page on XC REs

Getting started – A few things to do first.

Have the VS Code IDE
-	Install the Docker extension
-	Have git installed to pull and push code around on your laptop
Install the docker desktop on your workstation (Windows and MAC available) – you need the service, not too much to do with the UI (might be a better way to just install the service, but this works)
Create a container repository that you control. I’m using Azure Container registry - https://learn.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal?source=recommendations&tabs=azure-cli
Access to F5 XCS to create k8s deployments and http load balancers
Creating the k8s cluster, service, and http load balancing on a XC Regional Edge site  
Ref. https://clouddocs.f5.com/training/community/f5xc/html/class7/module1/lab2.html
Creating the virtual site and k8s cluster
We need to build a virtual site that is leveraging regional edges to host the application. The reference above explains the dashboard and how a customer edge is used. Here are the steps to create a RE virtual site
Using app stack service, locate virtual site on the left 

Enter in the following information for a RE deployed site 


Example of selecting a specific RE location 
Be mindful of the REs used to host the application. If pulling from a public registry like DockerHub, you will max out the daily allowed pulls from the repo.   
Select the label – ves.io/siteName (enter value for RE locations)


Select your RE
 
Create a virtual k8s
Back to Distributed Apps service, now select virtual k8s on the left menu panel 
 
Create your k8s cluster – only one virtual k8s is available to you without an AppStack purchase
 
Look we’re all becoming devOps engineers before our very own eyes. Bad security engineer joke.   
 



When finished, click on the k8s cluster
 
Review the site assignment. You now see the locations of REs assigned by the virtual site creation. Let’s create some pods. 
 
 
Deploy and Scale a k8s Service
Start back at Distributed Apps service, select Applications, and select Virtual k8s. Click on the virtual k8s cluster you just created. At the Dashboard, select the tab near the top of the screen called Workloads. 
 
Create k8s service 
In the k8s service screen, a few things to note. Create the workload and be sure to select SERVICE (not simple service). There are a few reasons associated to why of this selection. For this use case we want to control which REs will host the application, not deploys to all REs across the F5 infrastructure. 
 
Select Configure in the Spec menu to configure the service.
Select advanced fields on the right corner of the screen. We will want to configure the replicas to 2 on each RE we will have services deployed. 
Configure Containers 
 
There are a few things going on here that needs to be pointed out. 
-	You will need to use the username that you have configured to access your container repo in the name. My token name configured in Azure container registry is f5xctestapp1. 
-	Second, the image name needs to be the name that is in the repo to be pulled from. Additionally, first time running through the registry configuration and using a private registry, you will need to add your registry to XC. 
-	Third, I’ve selected the Always pull policy. I prefer this method because when I restart the service, XC will pull a fresh copy of the container from my repo. If I make changes to my container for the application, after I’ve pushed the container to my repo and restart service on XC, the changes will be reflected in my application. 
A finished container setup will look like this
 
Next, setup Deploy Options 
Select regional Edge Virtual Sites. Select Configure 
 
Select the site we created in the first section of this exercise. Click Apply
 
Next, we create the load balancer that will advertise the service we just created. Consider this an internal load balancer that we will leverage as an origin server. (ask MC for more understanding around this). 
 
 
Click Apply, then Save. You have created a k8s workload


