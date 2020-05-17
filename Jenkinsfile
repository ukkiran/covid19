node {
   stage('init') {
      checkout scm
    }
    stage('build') {
       sh '''
          mvn clean package
        '''
    }
    stage('Build image') {
       app = docker.build("vjytraining/covid19webappvijay") 
    }

    stage('push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'DOCKERHUB') {
            app.push("latest")
         }
     }
     
     stage('sonarqube analysis') {
         withSonarQubeEnv('SonarQube') {
            sh '''
               mvn sonar:sonar
              '''
            }
     }
 
}

