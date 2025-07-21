# Deploy a Java based Application with CICD in k8s using Jenkins, Docker, SonarQube and ArgoCD


In this project, i did an end to end deployment of a spring-boot-app in kubernetes cluster using; **ArgoCD**, **Jenkins**, **Docker pipeline**, **SonarQube**

## Project Overview


![argocd cicd](https://user-images.githubusercontent.com/43399466/228301952-abc02ca2-9942-4a67-8293-f76647b6f9d8.png)



The Project is divided into two sections; **Continous Integration(CI)** and **Continous Deployment(CD)**

Below are the steps taken to complete this project.

## STEPS for Continous Integration(CI)
1. Launch an ubuntu ec2 machine and ssh into it.
1. update package manager
2. install java openjdk-11
3. add and save jenkins package sign-in key
4. add and save jenkins package repository
5. update package manager
6. install jenkins 
7. locate the jenkins admin password
8. access jenkins with server public IP on port 8080
9. enter admin password and allow suggested plugin download
10. setup username and password (jenkins default username:password is admin:admin)
11. navigate to manage jenkins and select plugins
12. add docker pipeline and sonarqube scanner plugins from the available plugins tab
13. install and restart jenkins
14. on the cli, update the package manager and install docker so we can pull and use our maven image as our jenkins effemeral slave
15. after docker installation, navigate to root and add jenkins and ubuntu (in this case) users to the docker group and restart docker daemon
16. Create a sonarqube user who own sonarqube package in the server
17. Change to our sonarqube user and install sonarqube for scanning our code
18. we can now access sonarqube on the web browser and login with admin/admin…thereafter you can change your username and password…access is via server publicIP on port 9000.
19. now lets integrate our SCM (github), our container registry (dockerhub) and our code analysis tool (sonarqube) with jenkins…restart jenkins after this.
    
## we can now build our jenkins pipeline using the pipeline option

![Screenshot 2024-02-18 053826](https://github.com/dandiggle23/java-maven-sonar-argocd-helm-k8s/assets/64781879/0f62d099-a4f1-45d6-89f3-dc1208c41ad3)

20. we will also update our jenkinsfile to reflect the changes
e sonarqube ip address before we run the job.
21. now lets run the job… our expected outcome is the final artifact is containerized as a docker image and pushed to dockerhub with the build number as the tag.
22. we should also check our SCM (github) repo to confirm that the manifest file has been updated to reflect the new image tag.





## Check the sonar server to see if the sonar configuration has passed
<img width="957" alt="Screenshot 2024-02-18 043920" src="https://github.com/dandiggle23/java-maven-sonar-argocd-helm-k8s/assets/64781879/441a1ee5-8372-4fc1-91ea-40b0343292bd">

Yes it has.


## Set up ArgoCD on Kubernetes cluster to deploy our spring boot app
Setup Minikube for local setups and install Argo CD on your Kubernetes cluster using a Kubernetes operator. i.e this makes it fast and easy

## Create Application

Click on create application in the ArgoCD home page and enter **Application name**, **Project name**, enter 'Automatic' for **SYNC POLICY**, scroll down and provide the **Repository URL, Path, Cluster URL** and enter default for **NAMESPACE**, then click **Create**
![Screenshot 2024-02-18 052353](https://github.com/dandiggle23/java-maven-sonar-argocd-helm-k8s/assets/64781879/9e5550eb-a389-4a2f-979c-cb4ba7eb9547)


So there you have it, our application is deployed.

## Run the commands below to see the deployments and pods

```
kubectl get deploy
kubectl get pod
```

![Screenshot 2024-02-18 052419](https://github.com/dandiggle23/java-maven-sonar-argocd-helm-k8s/assets/64781879/8d434206-4619-45ef-8370-0612c08cdc87)

![Screenshot 2024-02-18 054236](https://github.com/dandiggle23/java-maven-sonar-argocd-helm-k8s/assets/64781879/cee45074-0eba-4127-9def-0992e5190de3)

They are all running
