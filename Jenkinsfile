pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/anliouJr/gestion_note.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'composer install'
            }
        }

        stage('Run spider') {
            steps {
                bat 'C:\\Users\\anlio\\OneDrive\\Bureau\\M1\\TEST LOGICIEL\\test zap \\spider.py'
            }
        }

        stage('Run Scan active') {
            steps {
                bat 'C:\\Users\\anlio\\OneDrive\\Bureau\\M1\\TEST LOGICIEL\\test zap \\scan_actif.py'
            }
        }

        stage('Run form_authentication') {
            steps {
                bat 'C:\\Users\\anlio\\OneDrive\\Bureau\\M1\\TEST LOGICIEL\\automatiser\\test_selenium.py'
            }
        }


        stage('Run Tests') {
            steps {
                // Génération d'un rapport XML de test
                bat 'vendor\\bin\\phpunit --log-junit test-results.xml'
            }
        }
    }
    post {
        always {
            // Récupération du rapport de test généré
            junit 'test-results.xml'
        }
    }
}

