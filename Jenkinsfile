pipeline{

    agent any

    parameters {
      choice choices: ['chrome', 'firefox'], description: 'Choose the browser', name: 'BROWSER'
      choice choices: ['aqa', 'qa'], description: 'Choose the Env for tests', name: 'ENV'
      booleanParam(defaultValue: false, description: 'Check this to skip stages', name: 'SKIP_STAGES')
    }

    stages{
        stage('Start Grid'){
            when {
                expression { return !params.SKIP_STAGES }
            }
            steps{
                sh 'docker-compose -f grid.yaml up -d'
            }
        }

        stage('Run tests'){
            when {
                expression { return !params.SKIP_STAGES }
            }
            steps{
                sh 'docker-compose -f tests.yaml up'
            }
       }
    }

    post{
        always{
            script {
                if (!params.SKIP_STAGES) {
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
                } else {
                    currentBuild.displayName += " (stages skipped)"
                }
            }
        }
    }
}