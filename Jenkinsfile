pipeline {
    agent any
    
    environment {
        CI = 'true'
        NODE_OPTIONS = '--openssl-legacy-provider'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building React App...'
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                echo 'Running Tests...'
                sh 'chmod +x jenkins/scripts/test.sh'
                sh './jenkins/scripts/test.sh'
            }
        }

        // KRITERIA 4: Manual Approval Stage
        stage('Manual Approval') {
            steps {
                input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
            }
        }

        // KRITERIA 2: Deploy Stage
        stage('Deploy') {
            steps {
                echo 'Deploying Application...'
                sh 'chmod +x jenkins/scripts/deliver.sh'
                sh 'chmod +x jenkins/scripts/kill.sh'
                
                // Menjalankan aplikasi
                sh './jenkins/scripts/deliver.sh'
                
                script {
                    // KRITERIA 3: Menjeda otomatis selama 1 menit (60 detik)
                    echo 'Aplikasi berjalan... Menunggu 1 menit sebelum otomatis dimatikan.'
                    sleep time: 1, unit: 'MINUTES'
                }
                
                // Mengakhiri aplikasi secara otomatis setelah 1 menit
                echo 'Waktu habis, mematikan aplikasi...'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }

    post {
        success {
            echo 'Pipeline Selesai dengan Sukses!'
        }
    }
}
