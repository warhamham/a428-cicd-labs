// node {
//     def dockerImage = 'node:16-buster-slim'

//     docker.image(dockerImage).inside('-p 3001:3000') {  // Menggunakan port 3001 di host
//         stage('Build') {
//             sh 'sudo npm install'  // Menambahkan sudo untuk npm install
//         }

//         stage('Test') {
//             sh 'sudo ./jenkins/scripts/test.sh'  // Menambahkan sudo untuk script test.sh
//         }

//         stage('Manual Approval') {
//             input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
//         }

//         stage('Deploy') {
//             sh 'sudo ./jenkins/scripts/deliver.sh'  // Menambahkan sudo untuk script deliver.sh
//             echo 'Aplikasi akan berjalan selama 1 menit...'
//             sleep 60  // Jeda selama 1 menit (60 detik)
//             echo 'Menghentikan aplikasi...'
//             sh 'sudo ./jenkins/scripts/kill.sh'  // Menambahkan sudo untuk script kill.sh
//         }
//     }
// }

pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
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
        stage('Deploy') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)' 
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}