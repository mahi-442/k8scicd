pipeline{
    agent any
    tools {
        maven 'maven3'
    }
    environment {
        DOCKER_TAG = getVersion()
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
        stage('docker build'){
            steps{
                sh 'docker build -t hueapp/mahi9618:${DOCKER_TAG} .'
            }
        }
    }
}
def getVersion(){
    def latestCommit = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return latestCommit
}