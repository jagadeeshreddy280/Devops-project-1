# Devops-project-1
Any Queries related
---

Gmail : jagadeeshbhavanam@gmail.com
---

linkedin : https://www.linkedin.com/in/bhavanam-jagadeeswara-reddy-1b85801b6
---


Deploy an Application on Nomad Workload cluster using CI/CD
---
pre-requisite:-
---
1. cloud platform       : AWS(IAM,EC2,VPC)
2. SCM                  : Github
3. CI/CD                : Jenkins
4. Container platform   : DockerHub
5. Orchestration        : Nomad cluster

Add Github,Nomad,dockerhub plugins in jenkins

****Note:jenkins & nomad running on different ec2
         allow 4646 port for nomad & 8080 port for jenkins

jenkins installation:
---
ubuntu ec2: https://pkg.jenkins.io/debian-stable/

linux ec2: https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/


Nomad Installation :
---
linux:
```
sudo yum install -y yum-utils

sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo

sudo yum -y install nomad
```
ubuntu:
```
sudo apt-get update && \
sudo apt-get install wget gpg coreutils
```
``` 
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
```
```
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
```
Installing nomad 
```
sudo apt-get update && sudo apt-get install nomad
```
checking the version
```
nomad version
```
Agent need to Run
```
sudo nomad agent -dev \
  -bind 0.0.0.0 \
  -network-interface='{{ GetDefaultInterfaces | attr "name" }}'
```

export NOMAD_ADDR=http://localhost:4646

Check the Status of nomad
```
nomad node status
```

jenkins ec2:
---
Install jenkins,Docker,install nomad but dont run,git
Docker 
```
sudo apt install docker.io
```
Enableing the docker on ec2
```
sudo systemctl enable docker
```
Starting the docker on ec2
```
sudo systemctl start docker
```
Checking the status of docker 
```
sudo systemctl status docker
```
Checking the status of jenkins
```
sudo systemctl status jenkins
```
```
sudo apt-get update
```
Need to install java latest version
```
sudo apt install openjdk-11-jdk
```
```
java --version
```
<img width="615" alt="Screenshot 2024-04-05 113946" src="https://github.com/jagadeeshreddy280/Devops-project-1/assets/116871383/20d144be-a017-4463-90f2-3d0f5f7de128">


Grant permission for docker 
```
sudo chmod 666 /var/run/docker.sock
```

copy publicip:8080 on browser 
```
  username: admin
  
  password: cat /var/lib/jenkins/secrets/initialAdminPassword
```
![image](https://github.com/jagadeeshreddy280/Devops-project-1/assets/116871383/147aac4f-57af-4246-aeea-9ee087e03a47)

Add Cred in jenkins:
---
1.Go to settings-->developer settings-->token-->new token (tick all boxes) -->create
 
2. Copy token use as password
Jenkins: ghp_Lmxn7epc

<img width="934" alt="Screenshot 2024-04-05 140534" src="https://github.com/jagadeeshreddy280/Devops-project-1/assets/116871383/3db5cc4e-ad9a-425b-b162-d6fa6ff4b577">


Save Git & Docker & nomad token in Jenkins

Go to Manage jenkins --> click Manage Credentials --> click system

![image](https://github.com/jagadeeshreddy280/Devops-project-1/assets/116871383/4ca28177-15e9-4b38-9426-8cc93e16f1c8)


Go to Global credentials (unrestricted) --> Add cred

![image](https://github.com/jagadeeshreddy280/Devops-project-1/assets/116871383/318641d8-06e8-483d-8cc4-7641054dfb01)

nomad plugin & integration:
---

Go to manage jenkins--> plugins --> nomad

Go to manage jenkins --> clouds --> click nomad option --> save

![image](https://github.com/jagadeeshreddy280/Devops-project-1/assets/116871383/3183b5a6-26e9-431c-b4a6-493ba5893fdb)


copy nomad url and paste on Nomad API URL box --> test connection --> successfull

![image](https://github.com/jagadeeshreddy280/Devops-project-1/assets/116871383/54329e3e-7023-4ef6-a5e4-9c7675677796)


if multi-node nomad need token 
create a token on nomad --> click on Nomad ACl ADD dropdown --> click on secret text --> add token in secret --> ID as nomad token --> description as Nomad token--> save click on test connection it is succesfull. 

![image](https://github.com/jagadeeshreddy280/Devops-project-1/assets/116871383/dc365502-cbbc-4f71-a5cb-a1c42283124b)


Nomad ec2:
---
1.copy publicip:4646 on browser

![image](https://github.com/jagadeeshreddy280/Devops-project-1/assets/116871383/1eb4d4bd-32b5-4041-a86a-a4b92a60a899)


Trigger jenkins job automatically whenever any changes in GitHub:-
---
IN GITHUB:

1. Go to repository

2. Inside repo --> settings -->click webhooks -->click add webhooks

3. Copy Jenkins url like http://2.33.344.44:8080 and Add /github-webhook/

 Example:
```
http://35.154.114.196:8080/github-webhook/
```
4.	Add in Payload URL --> save it.

5.	Change anything or push any file into git repo we can see job is triggered automatically.

<img width="854" alt="Screenshot 2024-04-05 113646" src="https://github.com/jagadeeshreddy280/Devops-project-1/assets/116871383/b95749f9-5b65-4eb5-8e2d-bec46f53b3fa">


 
Build nomad job
---
1. Create a pipeline --> add jenkinsfile  --> click on build

![image](https://github.com/jagadeeshreddy280/Devops-project-1/assets/116871383/de23ef74-73b8-4502-b38d-418d7ee39b2e)
   

NOTE: Before building need to add plugins,save cred related to github,dockerhub,nomad. 
      add nomad pubilc ip:4646 on jenkins master configuration on node section.

Github links
---
```
https://github.com/jagadeeshreddy280/app-frontend
```
```
https://github.com/jagadeeshreddy280/app-nomad
```


Build Both the pipelines and check in Nomad UI.

Nomad ec2:
---

1. Copy publicip:4646 on browser

2. We can see  jobs are in Running state  

![image](https://github.com/jagadeeshreddy280/Devops-project-1/assets/116871383/92b94b86-16ed-4aa3-aaf4-7782529ed245)

3.copy public ip:80 --> Application Will open
