node {
    def application = "pythonapp"
    def dockerhubaccountid = "mhrtechie"

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }

    stage('Push image') {
        withDockerRegistry([ credentialsId: "dockerHub" ]) {
            app.push()
            app.push("latest")
        }
    }

    stage('Deploy') {
        sh ("docker run -d -p 3333:3333 ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }

    stage('Remove old images') {
        // Remove old docker images (e.g., "latest" tag)
        sh("docker rmi ${dockerhubaccountid}/${application}:latest -f")
    }
}
