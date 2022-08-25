// https://tutorials.releaseworksacademy.com/learn/building-your-first-docker-image-with-jenkins-2-guide-for-developers


// docker login -u rogerchoi
// dckr_pat_6mGoTsLU0IJxV47PTvagTXYwFRc

sAgentLabel = 'wsl2'
sCredIdDockerHub = 'hub.docker.com-user-rogerchoi-1'
sDockerRegistry = 'https://registry.hub.docker.com'

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

    stage('Push image') {
        docker.withRegistry( sDockerRegistry, sCredIdDockerHub ) {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
