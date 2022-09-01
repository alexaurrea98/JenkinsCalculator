pipeline {
    agent any
    tools {
        maven 'apache maven 3.6.3'
        jdk 'JDK 8'
    }
    stages {
        stage ('Clean') {
            steps {
                sh 'mvn -f /var/lib/jenkins/workspace/CalculatorPipe/JenkinsCalculator/pom.xml clean install'

            }
        }

        stage ('Build') {
            steps {
                sh 'mvn -f /var/lib/jenkins/workspace/CalculatorPipe/JenkinsCalculator/pom.xml compile install'
            }
        }

        stage ('Short Tests') {
            steps {
                sh 'mvn -f /var/lib/jenkins/workspace/CalculatorPipe/JenkinsCalculator/pom.xml -Dtest=CalculatorTest test install'
            }
        }

        stage ('Long Tests') {
            steps {
                sh 'mvn -f /var/lib/jenkins/workspace/CalculatorPipe/JenkinsCalculator/pom.xml -Dtest=CalculatorTestThorough test install'

            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml'
                }
            }
        }

        stage ('Package') {
            steps {
                sh 'mvn -f /var/lib/jenkins/workspace/CalculatorPipe/JenkinsCalculator/pom.xml package install'
                archiveArtifacts artifacts: 'src/**/*.java'
                archiveArtifacts artifacts: 'target/*.jar'
            }
        }

    }
}
