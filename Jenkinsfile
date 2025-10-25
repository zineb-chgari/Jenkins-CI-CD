pipeline {
    agent any
    environment {
        // Charge le token GitHub stocké dans Jenkins
        GITHUB_TOKEN = credentials('Github-token')
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📥 Cloning the repository...'
                // Cloner le dépôt GitHub avec le credential Jenkins
                git branch: 'main', 
                    url: 'https://github.com/zineb-chgari/Jenkins-CI-CD', 
                    credentialsId: 'Github-token'
            }
        }

        stage('Setup Python Virtual Environment') {
            steps {
                echo '⚙️ Setting up Python virtual environment...'
                // Commandes Windows (bat) — activation et installation
                bat """
                    python -m venv venv
                    call venv\\Scripts\\activate
                    python -m pip install --upgrade pip
                    python -m pip install -r requirements.txt
                    python --version
                """
            }
        }

        stage('Run Tests') {
            steps {
                echo '🧪 Running tests with pytest...'
                // Important : réactiver le venv dans chaque bloc bat
                bat """
                    call venv\\Scripts\\activate
                    pytest --maxfail=1 --disable-warnings -q
                """
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
