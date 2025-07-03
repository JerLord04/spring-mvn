pipeline {
    agent any
    tools{
        maven 'maven_3_9_10'
    }
    environment{
        DOCKER_HOME = '/usr/local/bin/docker'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/JerLord04/spring-mvn.git']]])
                sh 'mvn clean install'
            }
        }
    }
}