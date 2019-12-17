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
                   serverId: 'Arti1',
                  image: "docker.myartifactory.com/test-docker/alpine:latest",
                  targetRepo: 'docker-local',
                  buildName: "${JOB_NAME}",
                  buildNumber: "${BUILD_NUMBER}",
                )           
            }

        }

stage ('Publish build info') {
    steps {
                rtPublishBuildInfo (
                   // buildName: "${JOB_NAME}",
                   // buildNumber: "${BUILD_NUMBER}",
                    //buildName: 'MK',
                    //buildNumber: '48',
                    serverId: "Arti1"
                )
            }
        }
    }

}
