pipeline {
    agent {
        label 'devsecops-box'
    }
    environment {
            DOCKER_PASS = credentials('DOCKER_PASS')
            DOCKER_USER = credentials('GH_USER/PASS')
        }
    stages{
        stage("Verify Branch"){
            steps{
                echo "$GIT_BRANCH"
            }
        }
        stage("Docker Images"){
            steps{
                sh(script: 'docker images')

            }
        }
        stage("Docker Build"){
            steps{
                sh(script: 'docker build -t zazathomas/azure-voting-app:1.0 -f azure-vote/Dockerfile azure-vote/')

            }
        }
        stage("Run CI Tests"){
            steps{
                sh(script: 'pip3.12 install pytest && pytest ./tests/test.py')
            }
            post{
                always{
                    echo "====++++Running Tests++++===="
                }
                success{
                    echo "====++++Tests Passed++++===="
                }
                failure{
                    echo "====++++Tests Failed++++===="
                }
            }
        }
        stage("Docker Push"){
            steps{
                sh(script: 'echo $DOCKER_PASS docker login -u $DOCKER_USER_USR --password-stdin')
                sh(script: 'docker push zazathomas/azure-voting-app:1.0')
                sh(script: 'docker logout')
            }
        stage("Run Trivy Scan"){
            steps{
                sh(script: 'trivy --scanners vuln --severity CRITICAL --ignore-unfixed --exit-code 0 --format json --output trivy.json image python')
            }
        }}
    post {
            always {
                echo "====++++Removing Built Images++++===="
                sh(script: 'docker rmi -f zazathomas/azure-voting-app:1.0')
            }
        }
}
}
