pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                label 'linux'
            }
            steps {
                sh 'python3 client/setup.py sdist bdist_wheel'
                sh 'python3 server/setup.py sdist bdist_wheel'
                sh 'python3 web/setup.py sdist bdist_wheel'
            }
        }
    }
}