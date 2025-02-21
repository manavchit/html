
pipeline {
    agent any
    
    tools {
        nodejs 'nodejs'
    }
    
    stages {
        stage("Setup Dependencies") {
            steps {
                git branch: "main", url: "https://github.com/manavchit/9.1cj.git"
                bat "npm install --verbose --omit=optional"
            }
        }
        
        stage("Build Project") {
            steps {
                bat "npm run build"
            }
        }
        
        stage("Run Tests") {
            steps {
                bat "npm test -- --passWithNoTests"
            }
        }
        
        stage("Static Code Analysis") {
            steps {
                bat "npx eslint src"
            }
        }
          stage("Deploy"){
            steps{
                script{
                    def netlifySiteID = '8ed69f4e-a423-4bb6-b83f-e25890c100d5'
                    def netlifyAccessToken = 'nfp_fYaKF29EEguLXaxCXp6BG5Y7UwLLNLDw7224'
                    
                    bat "npm install netlify-cli --save-dev"
                    bat "npx netlify deploy --site ${netlifySiteID} --auth ${netlifyAccessToken} --dir ./build --prod"
                }
            }
        }
    }
}
