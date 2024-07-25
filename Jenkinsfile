pipeline {
  agent {
    label 'devsecops-box'
  }
    stages{
        stage("Verify Branch"){
            steps{
                echo "$GIT_BRANCH"
            }
        }
        stage("Docker Build"){
            steps{
                sh(script: 'docker images')
            }
        }
}
}
