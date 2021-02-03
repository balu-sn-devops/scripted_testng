def semanticVersion = "${env.BUILD_NUMBER}.0.0"
def packageName = "spboot-package_${env.BUILD_NUMBER}"
def version = "${env.BUILD_NUMBER}.0"
pipeline {
 agent none
 tools {
    	maven 'Maven'
 }
 stages {
  stage("Checkout"){
   agent any
   steps{
     checkout scm
     //sh 'mvn clean test'
   }
   
  }
  
  stage("Tests") {
   agent any
   steps {
    checkout scm
    sh 'mvn clean test'
    step([$class: 'Publisher', reportFilenamePattern: '**/testng-results.xml'])
    snDevOpsArtifact(artifactsPayload:"""{"artifacts": [{"name": "devops_scripted_demo.jar","version": "${version}","semanticVersion": "${semanticVersion}","repositoryName": "devops_scripted_demo"}],"stageName": "Tests"}""") 
    snDevOpsPackage(name: "sentimentpackage", artifactsPayload: """{"artifacts": [{"name": "devops_scripted_demo.jar","repositoryName": "devops_scripted_demo","version":"${version}"}]}""")
   }
  }
  stage('Deploy'){
   agent any
   steps{
     snDevOpsChange(changeRequestDetails: """
                  {
                     "setCloseCode":false,
                     "attributes":{
                        "requested_by":{
                           "name":"DevOps System"
                        },
                        "category":"Service",
                        //**
                        "assignment_group":{
                           "name":"Incident Management"
                        },
                        
                        "assigned_to": "63b4c2cd0bb23200ecfd818393673a95",
                        "risk":"4",
                        "business_service":{
                           "name":"Corpsite Application"
                        },
                        **//
                        "start_date":"2021-02-02 08:00:00",
                        "end_date":"2021-02-02 08:00:00".  
                     }
                  }
                  """)     
   }
  }
 }
}
