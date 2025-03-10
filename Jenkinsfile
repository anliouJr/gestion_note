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

        stage('Static Code Analysis') {
            steps {
                script {
                    def exitCode = bat(returnStatus: true, script: '''
                        vendor\\bin\\phpstan analyse --level=max --no-progress --error-format=table --memory-limit=2G .
                    ''')
                    if (exitCode != 0) {
                        echo "⚠️ PHPStan a détecté des erreurs, mais le pipeline continue."
                    }
                }
            }
        }

        stage('Run Spider') {
            steps {
                bat '"C:\\Users\\anlio\\OneDrive\\Bureau\\M1\\TEST LOGICIEL\\test zap \\spider.py"'
            }
        }

        stage('Run Scan Active') {
            steps {
                bat '"C:\\Users\\anlio\\OneDrive\\Bureau\\M1\\TEST LOGICIEL\\test zap \\scan_actif.py"'
            }
        }

        stage('Run Form Authentication') {
            steps {
                bat '"C:\\Users\\anlio\\OneDrive\\Bureau\\M1\\TEST LOGICIEL\\automatiser\\test_selenium.py"'
            }
        }

        stage('SQLMap Scan') {
            steps {
                script {
                    def sqlmapCmd = 'sqlmap -u "http://localhost/restaurant/" --batch --output-dir="sqlmap_results" --level=3 --risk=2 --dbs > sqlmap_results/sqlmap_report.txt'
                    def sqlmapResult = bat(script: sqlmapCmd, returnStatus: true)

                    if (sqlmapResult == 0) {
                        echo "✅ Aucune vulnérabilité SQL détectée."
                    } else {
                        echo "⚠️  Des vulnérabilités SQL ont été détectées !"
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                bat 'vendor\\bin\\phpunit --log-junit test-results.xml'
            }
        }
    }

    post {
        always {
            junit 'test-results.xml'
            
            emailext (
                to: 'anlioujunior12@gmail.com',
                subject: "Résultat du pipeline - ${currentBuild.fullDisplayName}",
                body: """Bonjour,

                Le pipeline s'est exécuté avec le statut : ${currentBuild.currentResult}
                Vous pouvez consulter les résultats ici : ${env.BUILD_URL}

                **Rapport SQLMap** :
                ${readFile('sqlmap_results/sqlmap_report.txt')}
                """,
                attachLog: true
            )
        }
    }
}
