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
    }
}