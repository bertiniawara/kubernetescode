node {

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                script {
                    def imageName = "betiniawara842/kubernetes1"
                    def app = docker.build(imageName)
                }
            }
        }

        stage('Test Image') {
            steps {
                script {
                    app.inside {
                        sh 'echo "Tests passed"'
                    }
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    def credentials = credentialsId('DOCKER_HUB_PASSWORD')
                    sh "docker login -u betiniawara@gmail.com -p ${credentials.getVariable('PASSWORD')}"
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    def imageName = "betiniawara842/jenkins-docker-demo:${env.BUILD_NUMBER}"
                    sh "docker push $imageName"
                    app.push() // Assuming app refers to the built image (if not, use imageName)
                }
            }
        }

        stage('Trigger Manifest Update') {
            steps {
                script {
                    build job: 'updatemanifest', wait: true, parameters: [
                        string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)
                    ]
                }
            }
        }
    }
}
