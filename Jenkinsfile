pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Machine123456/docker-spring-boot.git']])
            }
        }
        
        stage("Build JAR") {
            steps {
                sh "cd ./sampleSpringBootWebApp && mvn clean install"
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    dockerImage = docker.build("andrealao/sample-service:latest")
                }
            }
        }
        
        stage ("Push to Docker Hub") {
                steps {
                    script{
                        withDockerRegistry([ credentialsId: "dockerHubAccont", url: "" ]) {
                            dockerImage.push("$BUILD_NUMBER")
                        }
                    }
                }   
        }
    }
}
