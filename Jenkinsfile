pipeline {
    agent any

    stages {
        stage('SCM Checkout') {
            steps {
                git 'https://github.com/Krishnakanthk12/springboot.git'
            }
        }

        stage('Compile-package') {
            steps {
                script {
                    def mvnHome = tool name: 'maven_build', type: 'maven'
                    sh "${mvnHome}/bin/mvn clean package"
                }
            }
        }

        stage('Build docker image') {
            steps {
                sh 'docker build -t krishnakanthk12/hello:0.0.1 .'
            }
        }

        stage('Docker Image Push') {
            steps {
                withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
                    sh "docker login -u krishnakanthk12 -p ${dockerPassword}"
                }
                sh 'docker push krishnakanthk12/hello:0.0.1'
            }
        }

//        stage('Docker Deployment') {
//            steps {
//                sh 'docker stop springboot-container || true'
//                sh 'docker rm springboot-container || true'
//                sh 'docker run -d -p 8081:8090 --name springboot-container krishnakanthk12/hello:0.0.1'
//            }
//        }

        stage('Deploy on Kubernetes') {
            steps {
                sh 'kubectl apply -f /var/lib/jenkins/workspace/opsverse/pod.yaml'
                sh 'kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}
