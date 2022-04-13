def gitInfo = {} 

pipeline {
    agent any 
    stages{
        stage("check") {
            steps{
                script{
                    gitInfo = checkout scm
                    echo "$gitInfo.GIT_COMMIT"
                }
            }
        }
    }
}