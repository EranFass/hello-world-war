pipeline {
  agent any
  stages {
    stage('Checkout Code') {
      steps {
        sh 'git(url: \'https://github.com/EranFass/hello-world-war.git\', branch: \'dev\', changelog: true)'
      }
    }

    stage('Build Maven war') {
      steps {
        sh '''sh \'\'\'mvn compile
mvn clean package\'\'\''''
      }
    }

    stage('Sonarcloud - code analysis') {
      steps {
        sh 'sh \'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=EranFass_hello-world-war\''
      }
    }

    stage('Docker build & Tag') {
      steps {
        sh 'sh \'docker build -t hello-world:$BUILD_ID .'
      }
    }

    stage('') {
      steps {
        sh '''withDockerRegistry(credentialsId: \'nexusConnection\', url: \'http://127.0.0.1:8123/repository/docker-hosted/\') {
          sh \'\'\'docker tag hello-world:$BUILD_ID 127.0.0.1:8123/repository/docker-hosted/hello-world:$BUILD_ID
              docker push 127.0.0.1:8123/repository/docker-hosted/hello-world:$BUILD_ID
              \'\'\''''
        }
      }

    }
  }