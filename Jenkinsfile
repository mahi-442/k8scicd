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
                sh 'docker build -t mahi9618/hueapp:${DOCKER_TAG} .'
            }
        }
        stage('docker login && push'){
            steps{
                withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerHubPwd')]) {
                    // some block
                    sh 'docker login -u mahi9618 -p ${dockerHubPwd}'
                }
                sh 'docker push mahi9618/hueapp:${DOCKER_TAG}'
            }
        }
        stage('k8s deploy'){
            steps{
                sh 'chmod +x changeTag.sh'
                sh './changeTag.sh ${DOCKER_TAG}'
                sshagent(['kops']) {
                    // some block
                    sh 'scp -o StrictHostKeyChecking=no services.yml versionChanged.yml ec2-user@35.154.167.213: /home/ec2-user/'
                    script{
                        try{
                            sh 'ssh ec2-user@35.154.167.213 sudo kubectl apply -f .'

                        }catch(error){
                            sh 'ssh ec2-user@35.154.167.213 sudo kubectl create -f .'

                        }
                        
                    }
                }

            }
        }
    }
}
def getVersion(){
    def latestCommit = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return latestCommit
}