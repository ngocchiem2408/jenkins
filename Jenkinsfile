#!/usr/bin/env groovy
def project_home="/home/rm/project/workspace"
def Buildscripts_dir="/home/Buildscripts"
pipeline {

    agent {label 'build-192.168.9.136'}
  
  stages {
    stage('Clone') { 
            steps {
                echo "clean workspace"
                dir(project_home)
                {sh 'rm -rf *'}
                echo "check out source"
                dir(project_home)
                {
                sh 'git clone https://github.com/buildpack/sample-java-app.git'
                }
            }
        }
    stage('Build') {
      steps {
         dir(project_home + "/sample-java-app")
         {
            sh 'mvn clean package '
        }
      }
    }
    stage('Build Docker file') {
      steps {
         dir(project_home + "/sample-java-app/target")
         {
            sh 'rm -rf  /root/sample-0.0.1-SNAPSHOT.jar' 
            sh 'cp sample-0.0.1-SNAPSHOT.jar /root'
            
          
        }
        dir("/root")
        {
        sh 'docker build -t test/java-app .'
        }
      }
    }
    stage('Push to nexus') {
      steps {
         dir(project_home + "/sample-java-app")
         {
            sh 'docker login -u admin -p admin123 192.168.9.141:8083'
            sh 'docker tag test/java-app:latest  192.168.9.141:8083/test/java-app:1.0'
            sh 'docker push  192.168.9.141:8083/test/java-app:1.0'
            sh 'docker logout 192.168.9.141'
        }
      }
    }
    stage('Deploy to kubernetes') {
      steps {
         dir(project_home + "/sample-java-app")
         {
            echo 'deploy '
        }
      }
    }
  }
}
