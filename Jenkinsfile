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

        stage('Deploy') {
            sh 'npm run build'
            sh 'pwd'
            sh 'ls -la'

            sshagent(credentials: ['ec2-ssh-key']) {
                sh """
                    ssh -o StrictHostKeyChecking=no ${env.EC2_USER}@${env.EC2_HOST} \
                        'mkdir -p /home/ubuntu/react-app'
                """

                sh """
                scp -r -o StrictHostKeyChecking=no build ${env.EC2_USER}@${env.EC2_HOST}:/home/ubuntu/react-app
                """
            }
            // // sh './jenkins/scripts/deliver.sh'
            // input message: 'Finished using the website? (Click "Proceed" to continue)'
            // sh './jenkins/scripts/kill.sh'
        }
    }
}