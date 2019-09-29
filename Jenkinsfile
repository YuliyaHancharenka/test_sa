peline {
    agent { 
        label 'ubuntu'
    }
    environment {
        registry = 'dockeryuliya/test_sa'
        registryCredential = 'docker-hub-credentials'
    }
    stages {
        stage ("Clone repo") {
            steps {
                git url:'git@github.com:YuliyaHancharenka/test_sa.git',
                credentialsId: '66f6eb3b-3cd3-492c-997b-6579c2ab1049'
            }
        }
        stage ("Build image") {
            steps {
                script {
                    dockerImage = docker.build registry + ":${BUILD_NUMBER}"
                }
            }
        }
        stage ("Push image to DockerHub") {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage ("Remove image") {
            steps {
                sh "docker rmi ${registry}:${BUILD_NUMBER}"
            }
        }
    }
    post{
        success{
            slackSend (color: '#00CC00', message: "Job ${env.JOB_NAME} finished successfully. Build: ${env.BUILD_NUMBER}")
        }
        failure{
            slackSend (color: '#FF0000', message: "Job ${env.JOB_NAME} failed. Build: ${env.BUILD_NUMBER}")
        }
    }
}
