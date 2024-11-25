pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Mirzaa89/sast-demo-app',
                    credentialsId: 'github-pat'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                # Pastikan Python tersedia
                python3 --version || exit 1
                
                # Buat virtual environment
                python3 -m venv venv || exit 1

                # Aktifkan virtual environment
                . venv/bin/activate || exit 1

                # Instal Bandit
                pip install --upgrade pip || exit 1
                pip install bandit || exit 1
                '''
            }
        }
        stage('SAST Analysis') {
            steps {
                sh '''
                # Aktifkan virtual environment
                . venv/bin/activate

                # Jalankan analisis Bandit
                bandit -f xml -o bandit-output.xml -r . || true
                '''

                // Simpan hasil analisis
                archiveArtifacts artifacts: 'bandit-output.xml', allowEmptyArchive: true
            }
        }
    }
    post {
        always {
            sh '''
            # Bersihkan virtual environment
            rm -rf venv || true
            '''
        }
    }
}
