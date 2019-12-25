pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "eureka:latest"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/marijajelicic/eureka'
            }
        }
        stage ('Docker') {
            when {
                branch 'master'
            }
            steps{
                script{
                    sh '''
                            docker image build -t eureka:latest .
                        '''
                  // def app=docker.build(DOCKER_IMAGE_NAME)
                }
            }
            post {
                failure {
                    emailext (
                        subject: "Job '${env.JOB_NAME} ${env.BUILD_NUMBER} ",
                        body: """CI/CD pipeline greska u "Docker" fazi. Log fajl se moze videti na: href=${env.BUILD_URL} """,
                        to: "marija.jelicic@netcast.rs",
                        from: "jenkins@jenkins.netcast.rs"
                    )
                }
            }
        }
        stage ('Deploy') {
            when {
                branch 'master'
            }
            steps{
                script{
                    sh '''
                            docker run -d --name eureka -p 8761:8761 eureka:latest
                        '''
                }
            }
            post {
                failure {
                    emailext (
                        subject: "Job '${env.JOB_NAME} ${env.BUILD_NUMBER} ",
                        body: """CI/CD pipeline greska u "Build" fazi. Log fajl se moze videti na: href=${env.BUILD_URL} """,
                        to: "marija.jelicic@netcast.rs",
                        from: "jenkins@jenkins.netcast.rs"
                    )
                }
            }
        }
    }
}

