pipeline{
    agent { label 'Jenkins-Agent'}
    tools{
        jdk 'Java17'
        maven 'Maven3'
    }
    environment{
            APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "mihirmodi2561"
            DOCKER_PASS = 'DockerHub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages{
        stage("Cleanup Worksapce"){
            steps{
                cleanWs()
            }
        }
        stage("Check out from SCM"){
            steps{
                git credentialsId: 'github', url: 'https://github.com/mihirmodi2561/Register-app-ci-cd'
            }
        }
        stage("Build Application"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("Test Application"){
            steps{
                sh "mvn test"
            }
        }
        stage("SonarQube Analysis"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'Sonar-Qube'){
                    sh "mvn sonar:sonar"
                    }
                }
            }
        }
        stage("Quality check"){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-Qube'
                }
            }
        }
        stage("Build & Push Docker Image"){
            steps{
                script{
                    docker.withRegistry('',DOCKER_PASS){
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                }
            }
        }
        }
            
    }
}    
