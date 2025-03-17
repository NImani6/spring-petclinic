pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1') // Every 3 minutes on Mondays
    }

    environment {
        MAVEN_HOME = tool 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url:'https://github.com/Nimani6/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Run Tests and Generate Coverage') {
            steps {
                bat 'mvn test'
                bat 'mvn jacoco:report'
            }
            post {
                always {
                    jacoco(execPattern: '**/target/jacoco.exec', 
                           classPattern: '**/target/classes', 
                           sourcePattern: '**/src/main/java', 
                           exclusionPattern: '**/target/test-classes')
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}
