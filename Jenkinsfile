pipeline{
    agent { label 'Jenkins-Agent'}
    tools{
        jdk 'Java17'
        maven 'Maven3'
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
    }
}    
