pipeline {
    agent any
    
    environment {
        PHP_HOME = 'C:/wamp64/bin/php/php8.3.0'
        COMPOSER_HOME = 'C:/composer'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install dependencies') {
            steps {
                script {
                    echo 'Installing dependencies'
                    bat '"C:/composer/composer" install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    echo 'Running tests'
                    // Ajouter la commande pour exécuter tes tests ici si nécessaire
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building the project'
                    // Ajouter ici la commande pour la phase de construction de ton projet si nécessaire
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying project'
                    // Ajouter ici la commande de déploiement si nécessaire
                }
            }
        }
    }
    
    post {
        always {
            echo 'Cleaning up...'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
