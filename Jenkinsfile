pipeline{

    agent any

    parameters {
      choice choices: ['chrome', 'firefox'], description: 'Choose the browser', name: 'BROWSER'
    }

    parameters {
      choice choices: ['aqa', 'qa'], description: 'Choose the Env for tests', name: 'ENVS'
    }

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
            archiveArtifacts artifacts: 'allure-results/**', followSymlinks: false

             // Generate the Allure report
                        allure([
                            includeProperties: false,
                            jdk: '',
                            properties: [],
                            reportBuildPolicy: 'ALWAYS',
                            results: [[path: 'allure-results/**']]
                        ])
        }
    }
}