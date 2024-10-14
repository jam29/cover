pipeline {
    agent any

    tools {
        maven 'maven' // Your configured Maven installation
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jam29/cover.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package' // or './mvnw clean package' if using Maven wrapper
            }
        }

        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarq4j') { // Your SonarQube configuration
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('SonarQube Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                // Adjust this to your deployment strategy
                sh 'mvn spring-boot:run'
            }
        }
    }

    post {
        success {
            echo 'Build and SonarQube analysis succeeded!'
        }
        failure {
            echo 'Build or analysis failed.'
        }
    }
}
