node {
   stage('init') {
      checkout scm
   }
   stage('build') {
      sh '''
         mvn compile
         mvn test
         mvn clean package
       '''
   }
}
