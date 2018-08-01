pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh "./gradlew build"
            }
        }

        stage('Test') {
            steps {
                echo 'Testing..'
                sh "./gradlew test"
            }
        }
        stage('Deploy') {
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS'
              }
            }
            steps {
                echo 'Deploying....'
                sh "cf login -a https://api.local.pcfdev.io --skip-ssl-validation -u user -p pass -s pcfdev-test -o pcfdev-org && cf push"
            }
        }
    }
}