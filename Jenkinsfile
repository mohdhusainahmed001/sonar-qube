pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jaiswaladi246/Petclinic.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // 'SonarQube-Server' must match the name in Jenkins -> Manage Jenkins -> System -> SonarQube installations
                withSonarQubeEnv('SonarQube-Server') {
                    sh 'mvn sonar:sonar ' +
                       '-Dsonar.projectKey=Petclinic-Project ' +
                       '-Dsonar.projectName=Petclinic-Project ' +
                       '-Dsonar.language=java'
                }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // This waits for SonarQube to finish processing and report back
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
