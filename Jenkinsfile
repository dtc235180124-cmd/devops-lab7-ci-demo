pipeline {
    agent any

    tools {
        maven 'M3'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'STEP 1: CHECKOUT'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'STEP 2: BUILD'
                dir('sample-java-app') {
                    sh 'mvn clean compile'
                }
            }
        }

        stage('Test') {
            steps {
                echo 'STEP 3: TEST'
                dir('sample-java-app') {
                    sh 'mvn test'
                }
            }
            post {
                always {
                    junit 'sample-java-app/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'STEP 4: PACKAGE'
                dir('sample-java-app') {
                    sh 'mvn package -DskipTests'
                }
                archiveArtifacts artifacts: 'sample-java-app/target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'PIPELINE SUCCESS!'
        }
        failure {
            echo 'PIPELINE FAILED!'
        }
    }
}