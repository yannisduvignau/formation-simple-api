pipeline {
    agent any

    environment {
        GIT_REPO_URL = "https://github.com/yannisduvignau/formation-simple-api/"
        MAVEN_OPTS_BUILD = 'clean package -DskipTests'
        MAVEN_OPTS_TEST = '-Dmaven.test.failure.ignore=true test'
    }

    tools {
        maven "MAVEN_3_9_9"
    }

    stages {
        stage('Clone') {
            steps {
                git "${GIT_REPO_URL}"
            }
        }

        stage('Build') {
            steps {
                sh "mvn ${MAVEN_OPTS_BUILD}"
                sh "mvn ${MAVEN_OPTS_TEST}"
            }
            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}
