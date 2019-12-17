pipeline {

    agent any

    environment {


        DOCKER_REPO= "docker-local"

        DEBIAN_REPO = "debian-local"
        WORKSPACE ="/tmp/119353"

    }

    stages {
stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "Arti1",
                    url: "http://myartifactory.com/artifactory"
                )    
            }
        }
      stage("Pull docker images") {

            steps {

                script {

                    sh "docker pull alpine:latest"

                    sh "docker pull golang:latest"

                    sh "docker tag alpine:latest docker.myartifactory.com/test-docker/alpine:latest"

                    sh "docker tag golang:latest docker.myartifactory.com/test-docker/golang:latest"

                }

            }

        }


        stage("Push docker images") {

            steps {
                script{
                    def server = Artifactory.server "Arti1"

                    def rtDocker = Artifactory.docker server: server
                    rtDocker.push("docker.myartifactory.com/test-docker/alpine:latest", "${DOCKER_REPO}")
                    rtDocker.push("docker.myartifactory.com/test-docker/golang:latest", "${DOCKER_REPO}")

              //  rtDockerPush(
              //     serverId: "Arti1",
              //    image: "docker.myartifactory.com/test-docker/alpine:latest",
              //    targetRepo: 'docker-local',
              //    buildName: "119353",
              //    buildNumber: "21",
              //  )           
            }
            }

        }

stage ('Publish build info') {
    steps {
                rtPublishBuildInfo (
                    serverId: "Arti1"
                )
            }
        }
    }
}
