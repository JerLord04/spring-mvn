pipeline {
    agent any
    tools{
        maven 'maven_3_9_10'
    }
    environment{
        DOCKER_HOME = '/usr/bin/docker'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/JerLord04/spring-mvn.git']]])
                sh 'mvn clean install'
            }
        }

        stage('Build image'){
            steps{
                script{
                    sh '${DOCKER_HOME} build -t discovery-service:latest .'
                }
            }
        }

        stage('Push image to hub'){
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-pwd',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                sh '''
                    ${DOCKER_HOME} login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}
                    ${DOCKER_HOME} tag discovery-service:latest jerlord04/discovery-service:latest
                    ${DOCKER_HOME} push jerlord04/discovery-service:latest
                '''
                }
            }
        }

        stage('Deploy to k8s'){
            agent {
                docker {
                    image 'bitnami/kubectl:latest'
                }
            }
            steps{
                sh '''
                    kubectl
                '''
//               script{
//                  kubernetesDeploy (configs: 'k8s/deployment.yml',kubeconfigId: 'kubeconfig')
//                  kubernetesDeploy (configs: 'k8s/service.yml',kubeconfigId: 'kubeconfig')
//               }
            }

        }
    }
}