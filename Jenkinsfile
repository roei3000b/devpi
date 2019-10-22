pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                label 'linux'
            }
            steps {
                sh 'cd client && python3 setup.py sdist bdist_wheel'
                sh 'cd server && python3 setup.py sdist bdist_wheel'
                sh 'cd web && python3 setup.py sdist bdist_wheel'
            }
        }
        stage('Test') {
            agent {
                label 'linux'
            }
            steps {
                sh 'cd server && tox'
            }
        }
    }
}