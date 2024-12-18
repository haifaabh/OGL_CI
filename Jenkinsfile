pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat './gradlew build'
                //archiveArtifacts artifacts: '**/target/*.jar'
            }
        }
    }
}