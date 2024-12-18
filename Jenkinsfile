pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh './gradlew test'
                archiveArtifacts artifacts: '**/target/*.jar'
            }
        }
    }
}