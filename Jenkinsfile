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
       app = docker.build("vjytraining/covid19webapptracker") 
    }

     stage('push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'DOCKER HUB VIJAY') {
            app.push("latest")
         }
     }
   
}
