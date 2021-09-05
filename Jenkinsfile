pipeline{
    agent any
    tools {
        maven 'maven3'
    }
    stages{
        stage('git checkout'){
            steps{
                git credentialsId: 'github', url: 'https://github.com/mahi-442/k8scicd.git'
            }
        }
        stage('maven build'){
            steps{
                sh 'mvn clean package'
            }
        }
    }
}