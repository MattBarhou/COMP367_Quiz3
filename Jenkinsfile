pipeline {
    agent any

    triggers {
        cron('H/10 * * * 1') // Runs every 10 minutes on Mondays
    }

    tools {
        maven 'Maven 3' 
        jdk 'JDK 11'   
        

    environment {
        ARTIFACT_PATH = "target/spring-petclinic.jar"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/MattBarhou/COMP367_Quiz3.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Code Coverage') {
            steps {
                sh 'mvn jacoco:prepare-agent test jacoco:report'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                    jacoco execPattern: '**/target/jacoco.exec'
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Build succeeded! Artifact generated at ${ARTIFACT_PATH}"
        }
        failure {
            echo "Build failed!"
        }
    }
}
