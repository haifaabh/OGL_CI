pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://197.140.142.82:9000'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/haifaabh/OGL_CI.git'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                script {
                    try {
                        bat './gradlew test'
                        junit '**/build/test-results/test/*.xml'
                    } catch (Exception e) {
                        echo "Test stage failed: ${e.message}"
                        currentBuild.result = 'FAILURE'
                        error("Test stage failed")
                    }
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                script {
                    try {
                        bat "./gradlew sonarqube -Dsonar.host.url=${SONAR_HOST_URL}"
                    } catch (Exception e) {
                        echo "SonarQube analysis failed: ${e.message}"
                        currentBuild.result = 'FAILURE'
                        error("SonarQube analysis failed")
                    }
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                script {
                    try {
                        bat './gradlew build'
                        echo 'Generating Documentation...'
                        bat './gradlew javadoc' // Génération de la documentation
                        archiveArtifacts artifacts: '**/build/libs/*.jar', fingerprint: true
                        archiveArtifacts artifacts: '**/build/docs/javadoc/**', fingerprint: true

                   //     archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true

                    } catch (Exception e) {
                        echo "Build stage failed: ${e.message}"
                        currentBuild.result = 'FAILURE'
                        error("Build stage failed")
                    }
                }
            }
        }

        stage('Deployy') {
            steps {
                echo 'Deploying to MyMavenRepo...'
                bat "./gradlew publish"
            }
        }

        stage('Send Notification') {
            steps {
                script {
                    def result = currentBuild.result ?: 'SUCCESS'
                    if (result == 'SUCCESS') {
                        mail to: 'lh_bouhadi@esi.dz',
                             subject: "Jenkins Build #${env.BUILD_NUMBER} Success",
                             body: "The build #${env.BUILD_NUMBER} was successful.\n\nCheck it out: ${env.BUILD_URL}"
                    } else {
                        mail to: 'lh_bouhadi@esi.dz',
                             subject: "Jenkins Build #${env.BUILD_NUMBER} Failure",
                             body: "The build #${env.BUILD_NUMBER} failed.\n\nCheck it out: ${env.BUILD_URL}"
                    }
                }
            }
        }
    }


    post {
        always {
            echo 'Pipeline execution finished.'
        }

        success {
            echo 'Pipeline succeeded!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }

       stage('Slack Notification') {
          steps {
              slackSend channel: '#test',
              color: 'good',
              message: "Build ${env.JOB_NAME} #${env.BUILD_NUMBER} completed successfully."
                 }
             }

}