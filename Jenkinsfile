pipeline{

    agent any

    stages{

        stage('Git Checkout'){

            steps{

                git branch: 'main', url: 'https://github.com/GuttaDileep2160/ultimate_project.git'
            }
        }

        stage('Unit Testing'){

            steps{

                sh 'mvn test'                //Unit test command
            }
        }

        stage('Integration Testing'){

            steps{

                sh 'mvn verify -DskipUnitTests'                //Integration test command
            }
        }

        stage('Maven Build'){

            steps{

                sh 'mvn clean install'                //Maven build test command 
            }
        }

        stage('Static code analysis'){
            
            steps{
                
                script{
                    //sonar api is used by jenkisn to authenticatet to sonar server using credentialsid
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            }

        stage('Quality Gate Status'){
            steps{

                script{//quality gate analysis script developed by help of syntax generator

                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                }
            }
        }

        stage('Upload jar file to nexus'){
            steps{

                script{ //configure nexus upload plugin and then generate the syntax after configuring necessary things for synatx generator
                   
                    def readPomVersion = readMavenPom file: 'pom.xml'

                    def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "demojavaapp-snapshot" : "demojavaapp-release"

                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'springboot', 
                            classifier: '', 
                            file: 'target/Uber.jar', type: 'jar'
                            ]
                            ],
                             credentialsId: 'nexus-auth', 
                             groupId: 'com.example', 
                             nexusUrl: '44.211.238.49:8081', 
                             nexusVersion: 'nexus3', 
                             protocol: 'http', 
                             repository: nexusRepo, 
                             version: "${readPomVersion.version}"

                    
                }
            }
        }

        stage("Docker image Build"){
           
            steps{

                script{
                    sh 'docker build -t $JOB_NAME:v1.$BUILD_ID .'
                    sh 'docker tag $JOB_NAME:v1.$BUILD_ID gdileep/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker tag $JOB_NAME:v1.$BUILD_ID gdileep/$JOB_NAME:latest'
                }
            }
        }

        stage('Push the image to dockerHub'){

            steps{

                script{

                    withCredentials([string(credentialsId: 'docker_hub_creds', variable: 'docker_hub_cred')]) {
                        sh 'docker login -u gdileep -p ${docker_hub_cred}'
                        sh 'docker push gdileep/$JOB_NAME:v1.$BUILD_ID'
                        sh 'docker push gdileep/$JOB_NAME:latest'
                    }                    
                }
            }
        }
    }
}