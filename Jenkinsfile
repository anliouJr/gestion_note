pipeline {
    agent any

    environment {
        COMPOSER = 'C:/composer/composer'  // Chemin vers Composer sur ton hôte Windows
        PHP = 'C:/wamp64/bin/php/php8.3.0/php'  // Chemin vers PHP sur ton hôte Windows
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Utiliser Docker pour exécuter les commandes PHP et Composer
                    docker.image('php:8.3-cli').inside {
                        sh 'composer install'
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    docker.image('php:8.3-cli').inside {
                        sh 'phpunit --configuration phpunit.xml'
                    }
                }
            }
        }

        // Ajouter d'autres stages si nécessaire

    }

    post {
        always {
            cleanWs()
        }
    }
}
