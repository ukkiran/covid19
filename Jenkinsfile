pipeline {
    environment {
                registry = "ukkb96/covid19"
                registryCredential = 'dockerhub'
                dockerImage = ''
                jfrog_user="admin"
                jfrog_password="udaykiran@123"
    }
     agent { label 'master' }
        stages {
           stage('git checkout') {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/ukkiran/covid19.git']]])
            }
        }
        
          stage('build and package ') {
            steps{
               sh 'mvn clean package'
            }
         }
         stage('test') {
            steps{
                sh 'mvn test'
            }
         }
          stage('uploading artifacts to jfrog artifactory') {
            steps{
                   sh ' curl -X PUT -u ukkb96@gmail.com:Udaykiran@123 -T ./target/covid19-0.0.1-SNAPSHOT.jar "https://myjfrog9.jfrog.io/artifactory/maven-snapshots-local/covid19-0.0.1-SNAPSHOT.jar"'
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
                    sh 'docker push ukkb96/covid19":$BUILD_NUMBER"'
                  
                    }
                }
            }
         }
          stage('run application as an docker container on tomcat') {
            steps{
                sh 'docker run -dt --name covid19 -p 8085:8080 ukkb96/covid19":$BUILD_NUMBER"'
            }
         }
           stage('run codecoverage on sonarqube server') {
            steps{
                sh 'mvn sonar:sonar \
                         -Dsonar.projectKey=covid19pipeline \
                         -Dsonar.host.url=http://localhost:9001 \
                         -Dsonar.login=b7f72f36eca1e8d81010cbc33c4e1ef45efb7f1f'
                }
            }
          stage('run veracode scan') {
            steps{
                sh 'pwd'
            }
         }
    }
}
