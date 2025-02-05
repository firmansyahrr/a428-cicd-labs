node(null) {
    checkout scm

    docker.image('cimg/node:16.20').inside('-p 3000:3000 -u root') {
        stage('Build') {
            sh 'npm cache clean --force'
            sh 'npm install'
        }

        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }

        stage('Manual Approval') {
            input message: 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk melanjutkan)'
        }

        stage('Tes Connection') {
            sshagent(credentials: ['ec2-ssh-key']) {
                sh """
                    ssh -o StrictHostKeyChecking=no ${env.EC2_USER}@${env.EC2_HOST} \
                    'pwd'
                """
            }
        }

        stage('Deploy') {
            sh 'npm run build'
            sh 'pwd'
            sh 'ls -la'
            // // sh './jenkins/scripts/deliver.sh'
            // input message: 'Finished using the website? (Click "Proceed" to continue)'
            // sh './jenkins/scripts/kill.sh'
        }
    }
}