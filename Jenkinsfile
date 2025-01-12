// // declarative pipeline
// pipeline {
//     agent {
//         docker {
//             image 'node:16-buster-slim'
//             args '-p 3000:3000'
//         }
//     }
//     stages {
//         stage('Build') {
//             steps {
//                 sh 'rm -rf node_modules package-lock.json'
//                 sh 'npm install'
//             }
//         }
//         stage('Test') {
//             steps {
//                 sh './jenkins/scripts/test.sh'
//             }
//         }
//     }
// }

// // Scripted Pipeline
// node {
//     docker.image('node:16-buster-slim').inside('-p 3000:3000') {
//         stage('Build') {
//             sh 'npm install'
//         }
//         stage('Test') {
//             sh './jenkins/scripts/test.sh'
//         }
//     }
// }

node {
    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        stage('Build') {
            sh 'npm install'
        }

        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
    }

    stage('Manual Approval') {
        script {
            def userInput = input(
                message: 'Lanjutkan ke tahap Deploy?',
                parameters: [
                    choice(name: 'Approval', choices: ['Proceed', 'Abort'], description: 'Pilih Proceed untuk melanjutkan ke tahap Deploy atau Abort untuk menghentikan pipeline')
                ]
            )
            if (userInput == 'Abort') {
                error('Pipeline dihentikan oleh pengguna')
            }
        }
    }

    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        stage('Deploy') {
            sh 'npm start &'
            echo 'Aplikasi berjalan selama 1 menit...'
            sleep 60
            echo 'Tahap Deploy selesai.'
        }
    }
}

// pipeline {
//     agent any

//     environment {
//         DOCKER_IMAGE = 'node:16-buster-slim'
//         DOCKER_PORT = '3000:3000'
//         EC2_IP = '18.136.209.19'
//     }

//     stages {
//         stage('Build') {
//             steps {
//                 script {
//                     docker.image(DOCKER_IMAGE).inside("-p ${DOCKER_PORT}") {
//                         sh 'npm install'
//                     }
//                 }
//             }
//         }

//         stage('Test') {
//             steps {
//                 script {
//                     docker.image(DOCKER_IMAGE).inside("-p ${DOCKER_PORT}") {
//                         sh './jenkins/scripts/test.sh'
//                     }
//                 }
//             }
//         }

//         stage('Manual Approval') {
//             steps {
//                 script {
//                     def userInput = input(
//                         message: 'Lanjutkan ke tahap Deploy?',
//                         parameters: [
//                             choice(name: 'Approval', choices: ['Proceed', 'Abort'], description: 'Pilih Proceed untuk melanjutkan ke tahap Deploy atau Abort untuk menghentikan pipeline')
//                         ]
//                     )
//                     if (userInput == 'Abort') {
//                         error('Pipeline dihentikan oleh pengguna')
//                     }
//                 }
//             }
//         }

//         stage('Prepare Deploy') {
//             steps {
//                 script {
//                     // Exclude the app-files.tar.gz file and create the tarball
//                     sh 'tar --exclude=app-files.tar.gz -czf app-files.tar.gz .'
//                 }
//             }
//         }

//         stage('Deploy to EC2') {
//             steps {
//                 withCredentials([sshUserPrivateKey(credentialsId: 'aws-ec2-key', keyFileVariable: 'AWS_KEY')]) {
//                     script {
//                         sh '''
//                             # Upload files to EC2
//                             scp -o StrictHostKeyChecking=no -i $AWS_KEY app-files.tar.gz ubuntu@$EC2_IP:/home/ubuntu/

//                             # Deploy on EC2
//                             ssh -o StrictHostKeyChecking=no -i $AWS_KEY ubuntu@$EC2_IP << 'EOF'
//                                 cd /home/ubuntu
//                                 tar -xzf app-files.tar.gz
//                                 docker build -t react-app .
                                
//                                 # Stop and remove existing container if exists
//                                 if docker ps -a --format '{{.Names}}' | grep -q '^react-app-container$'; then
//                                     docker stop react-app-container
//                                     docker rm react-app-container
//                                 fi

//                                 # Run new container
//                                 docker run -d --name react-app-container -p 3000:3000 react-app
//                             EOF
//                         '''
//                     }
//                 }
//             }
//         }
//     }
// }
