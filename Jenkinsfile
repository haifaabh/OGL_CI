pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat './gradlew sonar'
                //archiveArtifacts artifacts: '**/target/*.jar'
            }
        }
    }
}