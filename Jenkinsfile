pipeline {
    agent any

    environment {
        GIT_REPO_URL = "https://github.com/yannisduvignau/formation-simple-api/"
        MAVEN_OPTS_BUILD = 'clean package -DskipTests'
        MAVEN_OPTS_TEST = '-Dmaven.test.failure.ignore=true test'
        MAVEN_VERSION = 'MAVEN_3_9_9'
        PROD_BRANCH = 'master'
    }

    tools {
        maven "${MAVEN_VERSION}"
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
            }
            post {
                success {
                    archiveArtifacts 'target/*.jar'
                }
                failure {
                    echo 'Build failed!'
                }
            }
        }

        stage('Tests') {
            when {
                expression { return fileExists('src/test/java') }
            }
            steps {
                sh "mvn ${MAVEN_OPTS_TEST}"
            }
            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
                failure {
                    echo 'Tests failed!'
                }
            }
        }

        stage('Deploy') {
            when {
                branch "${PROD_BRANCH}"
            }
            steps {
                echo 'Deploying to production...'
                // Add deployment steps here, e.g., upload to a server or cloud service
            }
            post {
                success {
                    echo 'Deployment successful!'
                }
                failure {
                    echo 'Deployment failed!'
                }
            }
        }
    }
}
