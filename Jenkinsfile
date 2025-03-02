pipeline {
    agent {
        docker {
            image 'php:8.3-cli'  // Utilise l'image Docker avec PHP et Composer préinstallés
            args '-v C:/Users/anlio:/mnt/project'  // Monte ton répertoire Windows à Docker (si nécessaire)
        }
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Récupérer le code depuis le dépôt Git
                    checkout scm
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...'

                    // Utiliser Composer pour installer les dépendances
                    sh '''#!/bin/bash
                    composer install
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    echo 'Running tests...'
                    // Ajoute ici tes commandes pour exécuter les tests
                    sh '''#!/bin/bash
                    # Exemple de commande pour PHPUnit
                    php vendor/bin/phpunit
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building project...'
                    // Ajoute ici les commandes nécessaires pour construire ton projet
                    // Par exemple, minifier des fichiers JS/CSS ou effectuer d'autres étapes de build
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying project...'
                    // Ajoute ici tes commandes pour déployer ton application
                    // Exemple de déploiement sur un serveur ou dans un dossier spécifique
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            // Actions à effectuer après chaque build, comme supprimer des fichiers temporaires
        }
        success {
            echo 'Build succeeded!'
            // Actions en cas de succès (par exemple, notifier l'équipe)
        }
        failure {
            echo 'Build failed!'
            // Actions en cas d'échec (par exemple, envoyer une alerte)
        }
    }
}
