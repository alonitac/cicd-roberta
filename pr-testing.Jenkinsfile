pipeline {
    agent any

    stages {
        stage('Unittest') {
            steps {
                sh '''
                pip3 install pytest
                python3 -m pytest --junitxml results.xml tests
                '''
            }
        }
        stage('Lint') {
            steps {
                sh 'echo "linting"'
            }
        }
        stage('Functional test') {
            steps {
                sh 'echo "testing"'
            }
        }
    }
}