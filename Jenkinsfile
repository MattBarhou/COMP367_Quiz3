pipeline {
    agent any

    triggers {
        cron('H/10 * * * 1') // Runs every 10 minutes on Mondays
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MattBarhou/COMP367_Quiz3.git'
            }
        }

        stage('Setup') {
            steps {
                sh 'echo "Setting up environment variables"'
                sh 'export JAVA_HOME=$(which java)'
                sh 'export PATH=$JAVA_HOME/bin:$PATH'
                sh 'mvn --version' // Check if Maven is accessible
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
                    junit '**/target/surefire-reports/*.xml'
                    jacoco execPattern: '**/target/site/jacoco/jacoco.xml'
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
            echo "Build succeeded! Artifact generated successfully."
        }
        failure {
            echo "Build failed! Please check logs for more details."
        }
    }
}
