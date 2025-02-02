node {
    checkout scm 

    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        stage('Build') {
            // sh 'npm install'
            echo 'Application will be active for 1 minutes'
        }
        
        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
    }
}