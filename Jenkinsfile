pipeline{

    agent any

    stages{
        stage('Run test'){
            steps{
                sh ' docker-compose -f grid-plus-tests.yaml up --scale firefox=2 --scale chrome=3'
            }
        }
        stage('Bring down the grid'){
            steps{
                sh 'docker-compose -f grid-plus-tests.yaml down'
            }
        }


    }
}