pipeline {
  agent any
  stages {
    stage('Checkout Code') {
      steps {
        git(url: 'https://github.com/EranFass/hello-world-war.git', branch: 'dev', changelog: true)
      }
    }

    stage('Build Maven war') {
      steps {
        sh '''mvn compile
mvn clean package'''
      }
    }

    stage('Sonarcloud - code analysis') {
      steps {
        sh '''mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=EranFass_hello-world-war -Dsonar.login=1b28ef3b7cc58035c804267914f32331ac8d4921
'''
      }
    }

    stage('Docker build & Tag') {
      steps {
        sh 'docker build -t hello-world:$BUILD_ID .'
      }
    }

    stage('Push docker image to Nexus') {
      steps {
        withDockerRegistry(credentialsId: 'nexusConnection', url: 'http://127.0.0.1:8123/repository/docker-hosted/') {
          sh '''docker tag hello-world:$BUILD_ID 127.0.0.1:8123/repository/docker-hosted/hello-world:$BUILD_ID
              docker push 127.0.0.1:8123/repository/docker-hosted/hello-world:$BUILD_ID
              '''
        }

      }
    }

  }
  post {
    success {
      slackSend(message: "Build deployed successfully - ${env.JOB_NAME} #${env.BUILD_NUMBER} - (${env.BUILD_URL}) ", channel: 'my_notifier', color: '#00FF00')
    }

    failure {
      slackSend(message: " Build failed - ${env.JOB_NAME} #${env.BUILD_NUMBER} - (${env.BUILD_URL}) ", channel: 'my_notifier', color: '#FF0000')
    }

  }
  triggers {
    pollSCM('* * * * *')
  }
}