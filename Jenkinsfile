pipeline{

    agent any

    stages{

        stage('Build Jar'){
            steps{
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Image'){
            steps{
                sh 'docker build -t=sanseyvich/automation-template:latest .'
            }
        }

        stage('Push Image'){
            environment{
                DOCKER_HUB = credentials('dockerhub-creds')
            }
            steps{
                sh 'echo ${DOCKER_HUB_PSW} | docker login -u ${DOCKER_HUB_USR} --password-stdin'
                sh 'docker push sanseyvich/automation-template:latest'
                sh "docker tag sanseyvich/automation-template:latest sanseyvich/automation-template:${env.BUILD_NUMBER}"
                sh "docker push sanseyvich/automation-template:${env.BUILD_NUMBER}"
            }
        }

    }

    post {
        always {
            sh 'docker logout'
        }
    }

}