pipeline {
    agent any

    stages {
        stage('Build') {

            steps { 
                withGradle {
                    sh './gradlew assemble'
                }
            }
            
            post {
                success {
                    archiveArtifacts 'build/libs/*.jar'
                }
            }
        }


        stage('Test') {

            steps { 
                withGradle {
                    sh './gradlew test'
                    sh './gradlew integrationTest'
                }
            }

            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'
                }
            }
        }
    }
}
