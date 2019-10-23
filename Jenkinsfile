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
                stash includes: '**/dist/*.whl', name: 'devpi'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/dist/*.whl', onlyIfSuccessful: true
                }
            }
        }
        stage('Test') {
            agent {
                label 'linux'
            }
            steps {
                sh 'cd server && tox'
                sh 'cd web && tox'
            }
            post {
                always {
                    junit 'server/results.xml'
                    junit 'web/results.xml'
                }
            }
        }

        stage('Deploy') {
            agent {
                label 'linux'
            }
            steps {
                unstash 'devpi'
                sh 'service devpi stop'
                sh 'rm -rf /mnt/data/backup'
                sh 'devpi-server --export /mnt/data/backup'
                sh 'find -iname devpi_server*.whl -exec pip install -U {} \\;'
                sh 'find -iname devpi_web*.whl -exec pip install -U {} \\;'
                sh 'service devpi start'
            }
        }
    }
}