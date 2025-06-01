pipeline {
  agent any     // Run on any available agent (Jenkins node)

  tools {
    maven 'Maven 3.9.9'  // Name of Maven tool in Jenkins
    jdk 'JDK 11'         // Name of JDK in Jenkins config
  }

  environment {
    JAR_NAME = 'weather-api-1.0-SNAPSHOT.jar'
  }

  stages {
    stage('Checkout') {
       steps {
                git branch: 'main', url: 'https://github.com/asamaranayake/weather-api.git'
            }
        }

    stage('Build') {
      steps {
        bat 'mvn clean package -DskipTests'
      }
    }

    stage('Test') {
      steps {
        bat 'mvn test'
      }
    }

    stage('Deploy') {
  steps {
    script {
      echo "Killing old version if running..."
      bat """
        for /f "tokens=2 delims==;" %%a in ('wmic process where "CommandLine like '%%$JAR_NAME%%'" get ProcessId /value') do (
            taskkill /PID %%a /F
        )
      """

      echo "Starting app..."
      bat """
        start java -jar target\\%JAR_NAME%
      """
    }
  }
}

  }

  post {
    success {
      echo 'Pipeline completed successfully!'
    }
    failure {
      echo 'Pipeline failed. Check logs.'
    }
  }
}
