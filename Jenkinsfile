pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub repository
                script {
                    def branch = 'main'
                    try {
                        git url: 'https://github.com/NavyaADevops/navya-repo.git', branch: branch
                    } catch (Exception e) {
                        echo "Failed to checkout branch '${branch}'. Checking for 'master' branch."
                        branch = 'master'
                        git url: 'https://github.com/NavyaADevops/navya-repo.git', branch: branch
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
        }
    }
}
