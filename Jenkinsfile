stage('Install dependencies') {
    steps {
        bat 'composer install'
    }
}

stage('Run Tests') {
    steps {
        bat 'vendor\\bin\\phpunit --configuration phpunit.xml'
    }
}
