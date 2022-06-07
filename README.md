![Build Status](https://codebuild.us-east-2.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoidDRkc2xFKzlVaVpRZXRiK1VrV3crbVNQMnJBNHdzQjQrVlQwaXdqVHdlYW94RU9jODN0R0I4WGJieEJLdjRldktxN3pCMWNTbW9YVUVPUmNzcVFCd0FjPSIsIml2UGFyYW1ldGVyU3BlYyI6Ikg0S29yNDFQUEVHMWVoN2wiLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=master)

# Amazon ECS Workshop

This is part of an Amazon ECS workshop at https://ecsworkshop.com

# 用途
  * For Jenkins 整合測試
  * For ArgoCD 整合測試
   
# 實作說明
* Jenkinsfile 實作
   * 使用 Agent jenkins-slave
   * 整合 jenkins build docker image 
   * Push image to AWS ECR
   
# Jenkinsfile 說明 
 ```
 Jenkinsfile : 整合 sonarqube 掃描，build image -> deploy to EKS
 Jenkinsfile-2 : build with parameters
 Jenkinsfile-3 : 使用 BRANCH_NAME 檢查只有 develop 才部署，僅能在 Jenkins MultiBranch pipeline 運作
 ```
 
# 來源 
* source Fork 來源 : [ecsdemo-nodejs](https://github.com/brentley/ecsdemo-nodejs)
* 參老來源 : [INSTALL ARGO CD](https://www.eksworkshop.com/intermediate/290_argocd/install/)
