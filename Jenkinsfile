
pipeline {
    agent any
    stages{
        stage('build') {
            agent {
                docker {
                    image 'node:18-alpine'
		    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('test'){
            steps {
                sh '''
                echo "test stage"
                test -f build/index.html
                npm test
                '''
            }
        }
    }
}