# createKBSCluster

This is the final stage in the CI/CD deployment for the Blue/Green deployment. It has the following stages:

####### Use this Repository files after you have created your infrastructure.Your 

Linting :-analyzes source code to flag programming errors
Build Image : Build your Docker image of your EC2 running OS|Kubernates|Jenkins|Git|Docker
Push Image To Dockerhub
Set Current kubectl Context
Deploy Blue Container
Deploy green container
Create Service Pointing to Blue Replication Controller
Approval for Redirection
Create Service Pointing to Green Replication Controller
