pipeline {
  agent any
  stages {
    stage('Generate Jar Files') {
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Delete the old containers') {
      steps {
        sh '''
          sudo systemctl stop backend-users || true
          sudo mkdir -p /var/lib/jenkins/backend/backend-users/
          sudo rm -rf /var/lib/jenkins/backend/backend-users/*
          sudo cp -r target/service-registry-0.0.1-SNAPSHOT.jar /var/lib/jenkins/backend/backend-users/
          sudo chown -R azureuser:azureuser /var/lib/jenkins/backend/backend-users/
          sudo mv /var/lib/jenkins/backend/backend-users/service-registry-0.0.1-SNAPSHOT.jar /var/lib/jenkins/backend/backend-users/app.jar
        '''
      }
    }
    stage('Run the updated dockers') {
      steps {
        sh 'sudo systemctl start backend-users'
      }
    }
  }
}
