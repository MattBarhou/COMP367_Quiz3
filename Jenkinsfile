pipeline {
    agent any

    tools {
        maven 'Maven 3'  // Use the configured Maven version
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MattBarhou/COMP367_Quiz3.git'
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
    always {
        junit '**/target/surefire-reports/*.xml'
        jacoco(
            execPattern: '**/target/jacoco.exec',  // Correct execution file
            classPattern: '**/target/classes',
            sourcePattern: '**/src/main/java',
            inclusionPattern: '**/*.class',
            exclusionPattern: '**/*Test*.class'
        )
    }
}

}
