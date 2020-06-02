
node('jenkins-ubuntu-slave') {
    stage('Checkout') {
        git branch: 'master', url: 'https://github.com/lRAHULl/demo-pipeline-project-1.git'
    }

    // stage('Code Analysis And Testing') {
    //     parallel {
    //         stage('Static Code Analysis') {
    //             sh 'chmod +x gradlew'
    //             // sh './gradlew checkstyle' 
    //         }

    //         stage('Unit Testing') {
    //             // sh './gradlew test'
    //         }
    //     }
    // }

    stage('Build') {
            // sh './gradlew bootjar'
            sh 'docker -t demo-pipeline build .'
    }

    stage('Deploy') {
            sh "docker tag demo-pipeline rahulraju/demo-pipeline-java-project-1:alpha-0.0.${BUILD_TAG}"
            sh 'docker push rahulraju/demo-pipeline-java-project-1'
    }

    stage('Clean') {
            sh 'docker rmi demo-pipeline'
    }
}

