pipeline {
    agent any

    environment {
        // Nombre del servidor SonarQube configurado en Jenkins
        SONARQUBE_SERVER = 'SonarQube'
    }

    tools {
        // Utilizar el SonarScanner gestionado por Jenkins
        sonarScanner 'SonarQubeScanner' // Nombre asignado en la configuración de herramientas
    }

    stages {
        stage('Checkout') {
            steps {
                // Clonar el código fuente
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    // Ejecutar el análisis con SonarScanner
                    sh "sonar-scanner -Dsonar.projectKey=testPipeLine -Dsonar.sources=vulnerabilities -Dsonar.php.version=8.0"
                }
            }
        }
        stage('Quality Gate') {
            steps {
                // Esperar el resultado del Quality Gate
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
