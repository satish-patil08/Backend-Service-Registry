pipeline {
    agent any

    stages {
        stage('Generate Jar Files') {
            steps {
                sh 'mvn clean install' // Build the Spring Boot project and generate the JAR file
            }
        }

        stage('Delete the old containers and deploy new JAR') {
            steps {
                sh '''
                # Stop the existing service
                sudo systemctl stop backend-notificationbackend-service-registry || echo "Service already stopped or does not exist"

                # Clean up the deployment directory
                sudo rm -rf /var/lib/jenkins/backend/backend-service-registry/*

                # Copy the new JAR file to the deployment directory
                sudo cp -r target/*.jar /var/lib/jenkins/backend/backend-service-registry/

                # Change ownership of the files to the appropriate user
                sudo chown -R azureuser:azureuser /var/lib/jenkins/backend/backend-service-registry/

                # Rename the JAR file to app.jar for consistency
                sudo mv /var/lib/jenkins/backend/backend-service-registry/*.jar /var/lib/jenkins/backend/backend-service-registry/app.jar
                '''
            }
        }

        stage('Run the updated service') {
            steps {
                sh '''
                sudo systemctl start backend-service-registry

                # Check the status of the service to ensure it started successfully
                sudo systemctl status backend-service-registry --no-pager
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded! The application has been deployed successfully.'
        }
        failure {
            echo 'Pipeline failed! Check the logs for errors.'
        }
        always {
            echo 'Cleaning up workspace...'
            cleanWs() // Clean up the Jenkins workspace
        }
    }
}