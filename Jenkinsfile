pipeline {
    agent any

    tools {
        // Noms identiques à ceux configurés dans Administrer Jenkins > Tools (Atelier 3)
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

          triggers {
        githubPush()
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                // Création du livrable (.jar) dans le dossier target
                sh 'mvn package -DskipTests'
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            echo 'Build réussi : livrable généré dans target/'
        }
        failure {
            echo 'Build échoué : voir les logs.'
        }
    }
}
