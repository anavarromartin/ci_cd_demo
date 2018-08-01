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
            }
        }
    }
}