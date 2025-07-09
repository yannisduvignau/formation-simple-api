pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MAVEN_3_9_9"
    }

    stages {
        stage('Build') {
            steps {
                git 'https://github.com/yannisduvignau/formation-simple-api/'
                
                // Run Maven on a Unix agent without tests.
                sh "mvn clean package -DskipTests"

                // Run Maven on a Unix agent without tests.
                sh "mvn -Dmaven.test.failure.ignore=true test"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}
