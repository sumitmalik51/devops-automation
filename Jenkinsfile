pipeline {
    agent any
    tools{
        maven 'maven_3_6_3'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sumitmalik51/devops-automation']]])
                sh 'mvn clean install'
            }
        }
         stage('Initialize'){
        def dockerHome = tool 'docker'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
    }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t sumitmalik51/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
                   sh 'docker login -u sumitmalik51 -p ${dockerhub}'

}
                   sh 'docker push sumitmalik51/devops-integration'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
