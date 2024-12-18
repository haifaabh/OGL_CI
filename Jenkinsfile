pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat './gradlew test'
                //archiveArtifacts artifacts: '**/target/*.jar'
            }
        }
    }
}