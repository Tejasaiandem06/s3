pipeline {
    agent any

    environment {
        function_name = 'jenkins'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Buildings'
                sh 'mvn package'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    // Run SonarQube analysis
                    sh 'mvn sonar:sonarcubeanalysis'
                }
            }
        }

        stage('Push') {
            steps {
                echo 'Push'

                sh "aws s3 cp target/sample-1.0.3.jar s3://sonartest12"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Build'

                sh "aws lambda update-function-code --function-name $function_name --region us-east-2 --s3-bucket jenkinsbucket28 --s3-key sample-1.0.3.jar"
            }
        }
    }
}
