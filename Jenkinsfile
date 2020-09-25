pipeline {
    environment {
                registry = "ukkb96/myapp"
                registryCredential = 'dockerhub'
                dockerImage = ''
    }
     agent { label 'master' }
        stages {
          stage('cloning github repository') {
             steps {
                sh 'sudo rm -rf covid19'
                sh 'git clone https://github.com/ukkiran/covid19.git'
             }
          }
         stage('Build') {
            steps {
                sh 'cd covid19'
                sh 'mvn clean package'
            }
         }
         stage('Test') {
            steps {
                sh 'mvn test'
            }
         }
         stage('deploy to tomcat') {
            steps{
               sh 'sudo cp ./target/covid19-0.0.1-SNAPSHOT.jar /opt/tomcat/webapps/'
            }
         }
         stage('Build image') {
            steps{
                script {
                     docker.build registry + ":$BUILD_NUMBER"
                }
            }
         }

         stage('push image') {
            steps{
                script{
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()}
                }
            }
         }
         stage('analysing code with sonarqube'){
            steps{
                sh 'mvn clean package sonar:sonar'
            }  
         }
    }
}
