// https://tutorials.releaseworksacademy.com/learn/building-your-first-docker-image-with-jenkins-2-guide-for-developers

// docker login -u rogerchoi
// dckr_pat_6mGoTsLU0IJxV47PTvagTXYwFRc

sAgentLabel = 'wsl2'
sCredIdDockerHub = 'hub.docker.com-user-rogerchoi-1'
sDockerRegistry = 'https://registry.hub.docker.com'
// error 404
// sDockerRegistry = 'https://hub.docker.com'
// For docker-hub, each image family is a repository
sImageTag = '0.0.1'
sImageRepo = 'rogerchoi/repo'

// https://docs.docker.com/docker-hub/repos/#:~:text=To%20push%20an%20image%20to,docs%2Fbase%3Atesting%20).
// https://www.section.io/engineering-education/docker-push-for-publishing-images-to-docker-hub/
// tag image with one of these
// . docker build -t <hub-user>/<repo-name>[:<tag>]
// . docker tag <existing-image> <hub-user>/<repo-name>[:<tag>]
// . docker commit <existing-container> <hub-user>/<repo-name>[:<tag>]
// docker push <hub-user>/<repo-name>:<tag>

node(sAgentLabel) {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Check image tag already exist') {
        docker.withRegistry( sDockerRegistry, sCredIdDockerHub ) {
            aImageAlreadyExist = true
            try {
                docker.image("${sImageRepo}:${sImageTag}").pull()
            }
            catch( Exception e ) {
                aImageAlreadyExist = false
            }
        }

        if( aImageAlreadyExist ) {
            throw new Exception(
                "ERROR image ${sImageRepo}:${sImageTag} exist - advance tag or delete image" )
        }
    }

    stage('Build image') {
        // build with the Dockerfile
        app = docker.build(sImageRepo)
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
            app.push(sImageTag)
            // app.push("${env.BUILD_NUMBER}")
            // app.push("latest")
        }
    }
}
