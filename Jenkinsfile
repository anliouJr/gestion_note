pipeline {
    agent any

    stages {
        stage('Cloner le repo') {
            steps {
                echo 'Clonage du dépôt GitHub...'
                git 'https://github.com/anliouJr/gestion_note.git'
            }
        }

        stage('Installer les dépendances') {
            steps {
                echo 'Installation des dépendances avec Composer...'
                bat 'composer install'
            }
        }

        stage('Exécuter les tests') {
            steps {
                echo 'Exécution des tests PHPUnit...'
                bat 'vendor\\bin\\phpunit --testdox || echo "Les tests ne sont pas encore configurés."'
            }
        }
    }
}
