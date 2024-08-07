pipeline {
    agent {
        label 'devsecops-box'
    }
    stages{
        stage("Verify Branch"){
            steps{
                echo "$GIT_BRANCH"
                echo "$GIT_URL"
            }
        }
       stage("Performing Secret Scanning Operations"){
            steps{
                sh(script: 'git clone https://github.com/GitGuardian/sample_secrets.git && cd sample_secrets/')
                sh(script: 'trufflehog filesystem ./ --no-verification --json > TRUFFLEHOG_RESULTS.json')
                sh(script: 'docker logout')
                sh(script: 'docker rmi -f $DOCKER_IMAGE')
            }}

    }
}
