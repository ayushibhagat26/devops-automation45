pipeline {
    agent any
    
    tools{
        maven 'maven_3_8_7'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ayushibhagat26/devops-automation45']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t ayushbhagat/devops-integration .'
                }
            }
        }

        
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u ayushbhagat -p ${dockerhubpwd}'

}
                   sh 'docker push ayushbhagat/devops-integration'
                }
            }
        }
        stage('Run Docker image') {
            steps {
                script {
                    sh 'docker pull ayushbhagat/devops-integration'
                    sh 'docker run -d -p 9092:8080 ayushbhagat/devops-integration'
                }
            }
        }
    }
}
