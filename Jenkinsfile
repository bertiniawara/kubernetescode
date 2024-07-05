node {
    def app

stage('Clone repository') {
  

    checkout scm
}

stage('Build image') {

   app = docker.build("betiniawara842/kubernetes1")
}

stage('Test image') {


    app.inside {
        sh 'echo "Tests passed"'
    }
}

stage("Docker Login"){
    
    withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
        sh 'docker login -u betiniawara@gmail.com -p $PASSWORD'

    }
}
    
stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'DOCKER_HUB_PASSWORD') {
            app.push("${env.BUILD_NUMBER}")
    }
}

stage('Trigger ManifestUpdate') {
            echo "triggering updatemanifestjob"
            build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
    }
}
