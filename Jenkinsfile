pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub repository
                script {
                    def branch = 'main'
                    try {
                        git url: 'https://github.com/jenkins-docs/simple-java-maven-app.git', branch: branch
                    } catch (Exception e) {
                        echo "Failed to checkout branch '${branch}'. Checking for 'master' branch."
                        branch = 'master'
                        git url: 'https://github.com/jenkins-docs/simple-java-maven-app.git', branch: branch
                    }
                }
            }
        }
        stage('Build') {
            steps {
                // Build the project
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                // Run the tests
                sh 'mvn test'
            }
            post {
                always {
                    // Publish test results
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
    post {
        success {
            // Trigger another job named "first" after success of all stages
            build job: 'first'

            // Send email notification on success
            emailext (
                subject: 'Pipeline Successful: ${currentBuild.fullDisplayName}',
                body: 'Good news! The pipeline ${env.JOB_NAME} [${env.BUILD_NUMBER}] has completed successfully.',
                to: 'navya2022developer@gmail.com'
            )
        }
    }
}

