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
    }
}