pipeline {
    agent any
    stages {
                stage('Checkout') {
            steps {
                git url: "https://github.com/siddeshringe/InsuranceManagement.git"
            }
        }

        stage('Maven Build') {
            steps {
                sh "mvn clean package"
            }
        }

        stage('Publish Test Reports') {
            steps {
                publishHTML(
                    reportDir: 'target/surefire-reports',
                    reportFiles: 'index.html',
                    reportName: 'HTML Report'
                )
            }
        }

        stage('Docker Image Build') {
            steps {
                echo 'Creating Docker image'
                sh "docker build -t ${dockerHubUser}/${containerName}:${tag} --pull --no-cache ."
            }
        }



        stage('Publishing Image to DockerHub') {
            steps {
                echo 'Pushing the docker image to DockerHub'
                withCredentials([usernamePassword(credentialsId: 'DockerHubAccount', usernameVariable: 'dockerUser', passwordVariable: 'dockerPassword')]) {
                    sh "docker login -u $dockerUser -p $dockerPassword"
                    sh "docker push ${dockerHubUser}/${containerName}:${tag}"
                    echo "Image push complete"
                }
            }
        }
    }
}
