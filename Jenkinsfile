pipeline{
   agent any

   environment {
      DOCKERHUB_CREDENTIALS=credentials('dockerhub-credentials')
   }

   stages {
      stage('Build') {
         steps {
            sh 'mvn install'
            sh 'docker build -t $DOCKERHUB_CREDENTIALS_USR/java-jenkins:latest .'
         }
      }

      stage('Login') {
         steps {
            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
         }
      }

      stage('Push') {
         steps {
            sh 'docker push $DOCKERHUB_CREDENTIALS_USR/java-jenkins:latest'
         }
      }
   }

   post {
      always {
         sh 'docker logout'
      }
   }
}
