// https://tutorials.releaseworksacademy.com/learn/building-your-first-docker-image-with-jenkins-2-guide-for-developers

sAgentLabel = 'wsl2'

node(sAgentLabel) {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        // build with the Dockerfile
        app = docker.build("releaseworks/hellonode")
    }

    stage('Test image') {
        app.inside {
            sh """
                echo "Tests passed"
            """
        }
    }

/*
    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
*/
}
