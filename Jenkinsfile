pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('Github-token')
    }

    triggers {
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
                        echo '🧪 Running test_app2.py...'
                        bat """
                            call venv\\Scripts\\activate
                            pytest test_app2.py --maxfail=1 --disable-warnings -q
                        """
                    }
                }
            }
        }
    }

    post {

        success {
            echo '✅ Pipeline completed successfully!'
            emailext(
                subject: "✔️ SUCCESS : ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    <h2 style='color:green;'>Pipeline Success</h2>
                    <p><b>Job :</b> ${env.JOB_NAME}</p>
                    <p><b>Build :</b> #${env.BUILD_NUMBER}</p>
                    <p><b>Logs :</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                to: "zinebchgari2004@gmail.com"
            )
        }

        failure {
            echo '❌ Pipeline failed!'
            emailext(
                subject: "❌ FAILURE : ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    <h2 style='color:red;'>Pipeline Failed</h2>
                    <p><b>Job :</b> ${env.JOB_NAME}</p>
                    <p><b>Build :</b> #${env.BUILD_NUMBER}</p>
                    <p><b>Logs :</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                to: "zinebchgari2004@gmail.com"
            )
        }

    }
}
