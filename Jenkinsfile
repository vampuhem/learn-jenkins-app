pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node-18-alpine'
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
        }
        stage('tests') {
            parallel
            stage('unit tests') {
                agent {
                    docker {
                        image 'node:18-alpine'
                        reuseNode true
                    }
                    steps {
                        sh '''
                        npm test
                        '''
                    }
                    post {
                        always {
                            junit 'test-results/junit.xml'
                        }
                    }
                }
            stage('e2e') {
                agent {
                    docker {
                        image 'node:18-alpine'
                        resueNode true
                    }
                }
                    steps{
                        sh '''
                            npm install serve
                            node_modules/.bin/serve -s build &
                            sleep 10
                            npx playwright test  --reporter=html
                        '''
                }
            }    
                    post {
                        always {
                            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                        }
                    }
            }
        }
    }
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
                steps {
                    sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    '''
                }
            }
        }
}        