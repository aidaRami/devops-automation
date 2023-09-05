pipeline {

    agent any
    tools{
        maven 'maven_3_5_0'
    }

    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ZouhairCharef/devops-automation']])
                sh 'mvn clean install'
            }
        }
        
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t zouhaircharef/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                       sh 'docker login -u zouhaircharef -p ${dockerhubpwd}'
                   }
                   sh 'docker push zouhaircharef/devops-integration'
                }
            }
        }
    }
    
}