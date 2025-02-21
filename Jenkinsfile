pipeline {
    agent any

    tools {
        nodejs 'nodejs'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    if (fileExists('package.json')) {
                        sh 'npm install --verbose --omit=optional'
                    } else {
                        error 'package.json not found, skipping npm install'
                    }
                }
            }
        }

        stage('Build Project') {
            steps {
                bat "npm run build"
            }
        }

        stage('Run Tests') {
            steps {
                bat "npm test -- --passWithNoTests"
            }
        }

        stage('Static Code Analysis') {
            steps {
                bat "npx eslint src"
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def netlifySiteID = '8ed69f4e-a423-4bb6-b83f-e25890c100d5'
                    def netlifyAccessToken = 'nfp_24YTxaUm5gk7D3Cdj9kUVkcQfRZ2BH3C9a91'

                    bat "npm install netlify-cli --save-dev"
                    bat "npx netlify deploy --site ${netlifySiteID} --auth ${netlifyAccessToken} --dir ./build --prod"
                }
            }
        }
    }
}
