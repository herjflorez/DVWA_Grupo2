pipeline {
    agent any

    environment {
        // Nombre del servidor SonarQube configurado en Jenkins
        SONARQUBE_SERVER = 'SonarQube'
        // URL del servidor SonarQube
        SONAR_HOST_URL = 'http://localhost:9000'
        // Token de autenticación para SonarQube, obtenido de las credenciales de Jenkins
        SONAR_AUTH_TOKEN = credentials('sonarQubeToken')
    }

    stages {
        stage('Checkout') {
            steps {
                // Clonar el código fuente desde el repositorio
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                // Configurar el entorno de SonarQube
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    // Ejecutar el análisis con SonarScanner
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=testPipeLine \
                        -Dsonar.sources=vulnerabilities \
                        -Dsonar.host.url=${SONAR_HOST_URL} \
                        -Dsonar.login=${SONAR_AUTH_TOKEN} \
                        -Dsonar.php.version=8.0
                    """
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
