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
      

        stage("Push docker images") {

            steps {

                rtDockerPush(
                   serverId: "Arti1",
                  image: "docker.myartifactory.com/test-docker/alpine:latest",
                  targetRepo: 'docker-local',
                  buildName: "119353",
                  buildNumber: "21",
                )           
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
