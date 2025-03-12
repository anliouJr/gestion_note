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
                bat '"C:\\Users\\anlio\\OneDrive\\Bureau\\M1\\TEST LOGICIEL\\test zap\\spider.py"'
            }
        }

        stage('Run Scan Active') {
            steps {
                bat '"C:\\Users\\anlio\\OneDrive\\Bureau\\M1\\TEST LOGICIEL\\test zap\\scan_actif.py"'
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
                    def sqlmapCmd = 'sqlmap -u "http://localhost/restaurant/login.php" --forms --crawl=2 --batch --dbs'
                    def sqlmapResult = bat(script: sqlmapCmd, returnStatus: true)

                    if (sqlmapResult == 0) {
                        echo "✅ Aucune vulnérabilité SQL détectée."
                    } else {
                        echo "⚠️ Des vulnérabilités SQL ont été détectées !"
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

            // Vérification des résultats de SQLMap
            def sqlmapResultFile = 'sqlmap_results/report.txt'
            def sqlmapReport = readFile(sqlmapResultFile).toLowerCase()

            def sqlmapVulnFound = sqlmapReport.contains("vulnerable") ? "SQLMap a trouvé des failles sur http://localhost/restaurant/login.php. Vérifie les logs dans Jenkins." : "Aucune vulnérabilité SQL détectée."

            // Envoi de l'email avec les informations du pipeline
            mail to: 'anlioujunior12@gmail.com',
                 subject: "[Jenkins] Exécution terminée : Pipeline gestion_note",
                 body: """Bonjour,

L'exécution du pipeline Jenkins est terminée.

- ✅ Résultat : ${currentBuild.result}
- 📅 Date : ${new Date()}
- 🔍 Consultez Jenkins pour plus de détails : ${env.BUILD_URL}

${sqlmapVulnFound}

Cordialement,
Jenkins
"""
        }
    }
}
