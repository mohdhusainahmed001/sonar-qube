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
                // -Djacoco.skip=true prevents the "IllegalClassFormatException" crash
                sh 'mvn clean test -Djacoco.skip=true'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // IMPORTANT: 'SonarQube-Server' must match the Name in 
                // Jenkins -> Manage Jenkins -> System -> SonarQube installations
                withSonarQubeEnv('SonarQube-Server') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=Petclinic-Project'
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                // This waits for SonarQube to finish processing the report
                waitForQualityGate abortPipeline: true
            }
        }
    }
}
