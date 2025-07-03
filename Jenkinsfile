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
            steps{
                sh '''
                    ${DOCKER_HOME} login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}
                    ${DOCKER_HOME} push discovery-service:latest
                '''
            }
        }
    }
}