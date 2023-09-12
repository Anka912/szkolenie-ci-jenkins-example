pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven-3"
        jdk "jdk-17"
    }

    stages {
        stage('Build') {
            steps {
                cleanWs()
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/spring-projects/spring-petclinic.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean verify"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit allowEmptyResults: true, testResults: '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                    slackSend channel: 'jenkins-ark', message: 'ble ble ble '
                }
            }
        }
    }
}

