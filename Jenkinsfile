pipeline {
    
    agent any {
        configFileProvider([configFile(fileId: 'hello-grails-gradle.properties')])
    }
     
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
                withGradle {
                    sh './gradlew clean test'
                    sh './gradlew iT'
                    sh './gradlew codenarcTest'
                }
            }

            post {
                always {
                    junit 'build/test-results/**/TEST-*.xml'
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
    }
}
