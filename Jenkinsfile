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
    post("Docker Remove Image"){
            always{
                sh(script: 'docker rmi -f zazathomas/azure-voting-app:1.0')

            }
        }
}
}
