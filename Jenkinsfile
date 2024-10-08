pipeline {
    agent {
        docker {
            image 'node:lts-buster-slim'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Approval'){
            input message: 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk melanjutkan ke tahapan deploy)'
        }
        stage('Deploy') { 
            steps {
                sh './jenkins/scripts/deliver.sh'
                echo 'Application will be active for 1 minutes'
                sleep(time: 1, unit: 'MINUTES')
                sh './jenkins/scripts/kill.sh' 
                echo 'Application killed'
            }
        }
    }
}
