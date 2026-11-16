pipeline{
    agent any

    stages{
        stage('Hello'){
            steps{
                echo 'Hello depuis un pipeline déclaratif'
            }
        }
        stage('Infos environnement'){
            steps{
                echo "Nom du noeud Jenkins: ${env.NODE_NAME}"
                echo "Workspace: ${env.WORKSPACE}"
            }
        }
        stage('Recuperation du code'){
            steps{
                checkout scm
            }
        }
        stage('Configuration Python'){
            steps{
                bat 'C:\Users\hp\AppData\Local\Programs\Python\Python311\python.exe --version'
                bat 'C:\Users\hp\AppData\Local\Programs\Python\Python311\python.exe - pip install uv'
            }
        }
        stage('Installation des dépendances') {
            steps {
                bat 'C:\Users\hp\AppData\Local\Programs\Python\Python311\python.exe -m uv sync --all-groups'
            }
        }
        stage('Exécution des tests') {
            steps {
                bat '''
                    mkdir -p reports

                    uv run pytest -q \
                        --junitxml=reports/tests.xml \
                        --cov=app \
                        --cov-report=term-missing \
                        --cov-report=xml:reports/coverage.xml \
                        --cov-fail-under=90
                '''
            }
        }
    }
    post {
        always {
            junit 'reports/tests.xml'
            archiveArtifacts artifacts: 'reports/coverage.xml',
                             allowEmptyArchive: true
        }
    }
}