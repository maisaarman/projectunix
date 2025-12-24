pipeline {
    agent any
    environment {
        // Set the container name and the workspace directory
        LAMP_CONTAINER_NAME = 'apache_container'
        LOCAL_PROJECT_FOLDER = "${WORKSPACE}/unixfinalproject"  // Point to the correct folder after checkout
    }
    stages {
        stage('Checkout') {
            steps {
                // Clone the GitHub repository
                git 'https://github.com/lojain-yadak/unixfinalproject.git'
            }
        }
        stage('Build') {
            steps {
                // If you need to install Composer or dependencies, you can do that here
                // For PHP-based apps, this step may not be necessary unless using frameworks.
                echo 'No build step required for PHP.'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Ensure the container is running before attempting to copy files
                    echo 'Starting containers...'
                    sh 'docker-compose up -d'  // Starts the containers in detached mode, if needed

                    // Copy files from the local repository to the Docker container
                    echo 'Deploying to Docker container...'
                    sh "docker cp ${LOCAL_PROJECT_FOLDER}/. ${LAMP_CONTAINER_NAME}:/var/www/html"
                }
            }
        }
        stage('Restart Apache') {
            steps {
                // Restart Apache to reflect the changes
                echo 'Restarting Apache...'
                sh 'docker exec apache_container service apache2 restart'
            }
        }
    }
    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Deployment was successful.'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
