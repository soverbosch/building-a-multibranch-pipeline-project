pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000 -p 5000:5000'
        }
    }

    parameters {
        choice(choices: ['services', 'volumes', 'secrets', 'all'], description: '', name: 'type')
        choice(choices: ['iam-am-server', 'iam-ds-config', 'iam-ds-cts', 'iam-ds-appstore', 'iam-master', 'all'], description: '', name: 'component')
    }

    environment {
        APP = 'iam-cleanup'
        TYPE= "${params.type}"
        COMPONENT= "${params.component}"
        CI = 'true'
    }

    stages {
        stage('Build') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Test') {
            steps {
                withEnv(['COMPONENT=XXXXX']) {
                    sh './jenkins/scripts/test.sh'
                }
            }
        }
        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
    }
}
