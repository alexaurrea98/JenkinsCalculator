pipeline {
    agent any
    tools {
        maven 'apache maven 3.6.3'
        jdk 'JDK 8'
    }
    environment {

            registry = "alexauf/jenkinscalc"

            registryCredential = 'dockerhub'

            dockerImage=''

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
                    junit allowEmptyResults: true, testResults: '**/test-results/*.xml'

                }
            }
        }

        stage ('Package') {
            steps {
                sh 'mvn -f /var/lib/jenkins/workspace/CalculatorPipe/JenkinsCalculator/pom.xml compile install'
                archiveArtifacts artifacts: '**/*.java'
                archiveArtifacts artifacts: 'JenkinsCalculator/target/*.jar'
            }
        }
        stage ('Building image') {
                    steps {
                        script {
                            dockerImage = docker.build registry + ":$BUILD_NUMBER"
                        }
                    }
        }
        stage ('Deploy Image') {
                    steps {
                        script {
                            docker.withRegistry('', registryCredential) {
                                dockerImage.push()
                            }
                        }
                    }

        }
        stage ('Remove unused docker image') {
                    steps {
                        sh "docker rmi $registry:$BUILD_NUMBER"
                    }
        }
    }

    post {
        failure{
            mail to: 'alexa.urrea98@gmail.com',
                subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                body: "Something is wrong with ${env.BUILD_URL}"

            }

    }
}
