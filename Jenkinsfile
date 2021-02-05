pipeline {
    agent {
        label "docker"
    }
    stages {
        stage ('Git Checkout') {
            steps {
                git "https://github.com/itay47/jnk-dkr-nexus.git"
            }
        }
        stage("Build docker image"){
            input {
                message "Enter build number"
                ok "Go!"
                submitter "Itay shechter"
                parameters {
                    string(name: 'BUILD_ID', defaultValue: 'latest', description: 'Build Number: x.y / string')
                }
            }
            steps{
                echo "====++++ building docker image ++++===="
                script{
                    sh 'docker build -t my-image:${BUILD_ID} .'
                }
                
            }
            post{
                success{
                    echo "====++++ build success ++++===="

                    script{
                        sh 'docker images'
                        sh 'docker ps'
                        sh 'docker run --rm --name my-image my-image:${BUILD_ID}'
                    }
                }
                failure{
                    echo "====++++ build execution failed ++++===="
                }
        
            }
        }
        stage("upload to nexus artifactory"){
            input {
                message "Upload to Nexus artifactory?"
            }
            steps{
                echo "====++++ uploading to nexus artifactory ++++===="

                sh 'docker push http://cicdvm:8081/my-image:${BUILD_ID}'
            }
        }
    }
}