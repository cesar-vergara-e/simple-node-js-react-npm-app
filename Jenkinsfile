pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
        KEY_ID     = credentials('jenkins-aws-secret-key-id')
        ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
//            agent {
//                docker { image 'andmyhacks/trufflehog' }
//            }
            steps {
//                sh 'trufflehog -v'
                sh 'echo $ACCESS_KEY'
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
