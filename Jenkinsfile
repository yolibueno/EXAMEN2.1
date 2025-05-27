pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME'  // Asegúrate de que este nombre coincide con lo configurado en Jenkins
    }

    environment {
        SONARQUBE = 'SonarQube-Local'  // Asegúrate de que así se llama tu servidor SonarQube en Jenkins
    }

    stages {
        stage('Clonar repositorio') {
            steps {
                git branch: 'main', url: 'https://github.com/yolibueno/EXAMEN2.1.git'
            }
        }

        stage('Compilar y Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('Análisis SonarQube') {
            steps {
                withSonarQubeEnv("${SONARQUBE}") {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Esperar Resultado de Calidad') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
