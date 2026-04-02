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
                // We add -Djacoco.skip=true to avoid the crash we saw in your logs
                sh 'mvn clean test -Djacoco.skip=true'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // This 'SonarQube-Server' name must match what you configured in Jenkins System settings
                withSonarQubeEnv('SonarQube-Server') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=Petclinic-Project'
                }
            }
        }
    }
}
