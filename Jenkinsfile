pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/anliouJr/gestion_note.git'
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'composer install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'vendor/bin/phpunit'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Build step (ajoute tes commandes ici)"'
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo "Deploy step (ajoute tes commandes ici)"'
            }
        }
    }

    post {
        success {
            echo 'Pipeline exécuté avec succès ! ✅'
        }
        failure {
            echo 'La pipeline a échoué ! ❌'
        }
    }
}
