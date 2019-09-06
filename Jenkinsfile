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
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                withEnv {
                    sh './jenkins/scripts/test.sh'
                }
            }
        }
        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deploy for production') {
            when {
                branch 'production'
            }
            steps {
                sh './jenkins/scripts/deploy-for-production.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
