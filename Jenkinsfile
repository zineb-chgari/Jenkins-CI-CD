pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('zineb_id')
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning the repository...'
                git branch: 'Master', url: 'https://github.com/zineb-chgari/Jenkins-CI-CD', credentialsId: 'zineb_id'
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                echo 'Setting up Python virtual environment...'
                bat '''
                    python -m venv venv
                    call venv\\Scripts\\activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                    python --version
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests with pytest...'
                bat '''
                    call venv\\Scripts\\activate
                    pytest --maxfail=1 --disable-warnings -q
                '''
            }
        }
    }

    post {
        success { echo '✅ Pipeline completed successfully!' }
        failure { echo '❌ Pipeline failed!' }
    }
}
