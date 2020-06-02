properties([pipelineTriggers([githubPush()])])
pipeline {
    agent {
        node {
            label 'linux-slave'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/lRAHULl/demo-pipeline-project-1.git'
            }
        }

        stage('Code Analysis And Testing') {
            parallel {
                stage('Static Code Analysis') {
                    steps {
                        sh 'chmod +x gradlew'
                        // sh './gradlew checkstyle'
                    }
                }

                stage('Unit Testing') {
                    steps {
                        echo "Testing"
                        // sh './gradlew test'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                // sh './gradlew bootjar'
                sh 'docker build -t demo-pipeline .'
                sendSlackMessage "Build Successul"
            }
        }

        stage('Publish') {
            steps {
                sh "docker tag demo-pipeline rahulraju/demo-pipeline-java-project-1:alpha-0.0.${BUILD_NUMBER}"
                sh 'docker push rahulraju/demo-pipeline-java-project-1'
                sendSlackMessage "Publish Successul"
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker service ls --filter name=simple-java --quiet | wc -l
                if [[ $SERVICES -eq 0 ]]; then
                    docker network rm simple-java || true
                    docker network create --driver overlay --attachable simple-java
                    docker service create --replicas 3 --network simple-java --name simple-java -p 8080:8080 rahulraju/demo-pipeline-java-project-1:alpha-0.0.${BUILD_NUMBER}
                else
                    docker service update --image rahulraju/demo-pipeline-java-project-1:alpha-0.0.${BUILD_NUMBER} simple-java
                fi
                '''
            }
        }

        stage('Clean') {
            steps {
                sh 'docker rmi demo-pipeline || true'
            }
        }

    }
}

void sendSlackMessage(String message) {
    slackSend botUser: true, channel: 'private_s3_file_upload', failOnError: true, message: "Message From http://3.231.228.54:8080/: \n${message}", teamDomain: 'codaacademy2020', tokenCredentialId: 'coda-academy-slack'
}


// node('jenkins-ubuntu-slave') {
//     stage('Checkout') {
//         git branch: 'master', url: 'https://github.com/lRAHULl/demo-pipeline-project-1.git'
//     }

//     // stage('Code Analysis And Testing') {
//     //     parallel {
//     //         stage('Static Code Analysis') {
//     //             sh 'chmod +x gradlew'
//     //             // sh './gradlew checkstyle' 
//     //         }

//     //         stage('Unit Testing') {
//     //             // sh './gradlew test'
//     //         }
//     //     }
//     // }

//     stage('Build') {
//             // sh './gradlew bootjar'
//             sh 'docker build -t demo-pipeline .'
//     }

//     stage('Deploy') {
//             sh "docker tag demo-pipeline rahulraju/demo-pipeline-java-project-1:alpha-0.0.${BUILD_TAG}"
//             sh 'docker push rahulraju/demo-pipeline-java-project-1'
//     }

//     stage('Clean') {
//             sh 'docker rmi demo-pipeline'
//     }
// }

