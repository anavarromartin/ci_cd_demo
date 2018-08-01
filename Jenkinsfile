pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh "./gradlew build && ./gradlew assemble"
            }
        }

        stage('Test') {
            steps {
                echo 'Testing..'
                sh "./gradlew test"
            }
        }
        stage('Deploy to test') {
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS'
              }
            }
            steps {
                echo 'Deploying....'
                sh "cf login -a https://api.local.pcfdev.io --skip-ssl-validation -u user -p pass -s pcfdev-test -o pcfdev-org && cf push -f manifest-test.yml"
            }
        }
        stage('Deploy to stage?') {
            steps {
                input 'Approve deployment?'
            }
        }
        stage('Deploying to stage') {
            steps {
                echo 'Deploying....'
                sh "cf login -a https://api.local.pcfdev.io --skip-ssl-validation -u user -p pass -s pcfdev-stage -o pcfdev-org && cf push -f manifest-stage.yml"
            }
        }
        stage('Deploy to prod?') {
                    steps {
                        input 'Approve deployment?'
                    }
                }
        stage('Deploying to prod') {
                steps {
                    echo 'Deploying....'
                    sh "cf login -a https://api.local.pcfdev.io --skip-ssl-validation -u user -p pass -s pcfdev-prod -o pcfdev-org && cf push -f manifest-prod.yml"
                }
        }
    }
}