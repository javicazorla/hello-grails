pipeline {
    
    agent any
     
    stages {
        stage('Build') {

            steps { 
                withGradle {
                    sh './gradlew assemble'
                }
            }
        }


        stage('Test') {
            
            steps {
                
                configFileProvider([configFile(fileId: 'hello-grails-gradle.properties', targetLocation: 'gradle.properties')]) {
                    sh './gradlew clean test'
                    sh './gradlew iT'
                    sh './gradlew codenarcTest'
                }
               /*
                withGradle {
                    sh './gradlew clean test'
                    sh './gradlew iT'
                    sh './gradlew codenarcTest'
                } */
            }

            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'
                    echo 'Publish Codenarc report'
                    publishHTML (
                        target: [
                            allowMissing           : false,
                            alwaysLinkToLastBuild  : false,
                            keepAll                : true,
                            reportDir              : 'build/reports/codenarc',
                            reportFiles            : '*.html',
                            reportName             : "Codenarc Report"
                        ]
                   )
                }
            }
        }
        
        stage('SonarQube analysis') {
            steps { 
                withSonarQubeEnv(credentialsId: '69314d91-0e1d-4709-9e60-2392f61f2e28', installationName: 'local') {
                    sh './gradlew sonarqube'
                }
            }
        }  

    }
}
