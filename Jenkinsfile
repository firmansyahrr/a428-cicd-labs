node {
    checkout scm 

    docker.image('node:18.18.2-alpine3.18').inside('-p 3000:3000') {
        stage('Build') {
            sh 'npm install'
        }
        
        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
    }
}