pipeline {
    agent any

    environment {
        registry = "211223789150.dkr.ecr.us-east-1.amazonaws.com/my-docker-repo"
    }
    stages {
        stage('Checkout') {
            steps {
                
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/akannan1087/docker-spring-boot']])
            }
        }
        
        stage("Build JAR") {
            steps {
                sh "cd .\sampleSpringBootWebAp && mvn clean install"
            }
        }

        
        stage ("Build Image") {
            steps {
                dockerImage = docker.build("andrealao/sample-service:latest")
            }
        }
        
        stage ("Push to Docker Hub") {
            steps {
                withDockerRegistry([ credentialsId: "dockerHubAccont", url: "" ]) {
                    dockerImage.push()
                }
        }
        
        stage ("Helm package") {
            steps {
                    sh "helm package springboot"
                }
            }
                
        stage ("Helm install") {
            steps {
                    sh "helm upgrade myrelease-21 springboot-0.1.0.tgz"
                }
            }
    }
}
