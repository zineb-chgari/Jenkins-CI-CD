pipeline {
    agent any

    environment {
        // Charge le token GitHub stocké dans Jenkins
        GITHUB_TOKEN = credentials('Github-token')
    }
    triggers { 
        // Déclenche le pipeline toutes les 5 minutes
        cron('H/5 * * * *')
    }
     triggers {
        // Déclenche le pipeline à chaque push sur le repo
        // Ceci fonctionne avec le webhook GitHub configuré dans Jenkins
        githubPush()
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📥 Cloning the repository...'
                git branch: 'main', 
                    url: 'https://github.com/zineb-chgari/Jenkins-CI-CD', 
                    credentialsId: 'Github-token'
            }
        }

        stage('Setup Python Virtual Environment') {
            steps {
                echo '⚙️ Setting up Python virtual environment...'
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
            when {
                not {
                    changeset "**/README.md"
                }
            }
            parallel {
                stage('Test App 1') {
                    steps {
                        echo '🧪 Running test_app.py...'
                        bat """
                            call venv\\Scripts\\activate
                            pytest test_app.py --maxfail=1 --disable-warnings -q
                        """
                    }
                }
                stage('Test App 2') {
                    steps {
                        echo '🧪 Running test_app_2.py...'
                        bat """
                            call venv\\Scripts\\activate
                            pytest test_app_2.py --maxfail=1 --disable-warnings -q
                        """
                    }
                }
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
