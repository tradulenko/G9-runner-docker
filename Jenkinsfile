pipeline{

    agent any

    stages{
        stage('Start Grid'){
            steps{
                sh 'docker-compose -f grid.yaml up -d'
            }
        }

        stage('Run tests'){
            steps{
                sh 'docker-compose -f tests.yaml up'
            }
       }


    }

    post{
        always{
            sh 'docker-compose -f tests.yaml down'
            sh 'docker-compose -f grid.yaml down'
        }
    }
}