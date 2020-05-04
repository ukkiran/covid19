import groovy.json.JsonSlurper

node {
    def container
    def acrSettings

    withCredentials([azureServicePrincipal('principal-credentials-id')]) {
        stage('Prepare Environment') {
            sh 'az login --service-principal -u 900df6e1-33f6-482a-adc1-26aaae82ca66 -p f9ece8c4-1318-41d7-93f2-0748ae9f67ef -t 5d7e8eda-4242-4b36-9fee-b7b1d41380c3'
            sh 'az account set -s 7b069e3e-2434-4f01-abf0-a960bf740edf'
            acrSettings = new JsonSlurper().parseText(
                                            sh(script: "az acs show -o json -n azacrtest", returnStdout: true))
        }
        stage('build') {
        sh '''
       	   mvn clean package
        '''
        } 

        stage('Build') {
            container = docker.build("azacrtest.azurecr.io/covid19test2")
        }

        stage('Publish') {
            /* https://issues.jenkins-ci.org/browse/JENKINS-46108 */
            sh "docker login -u ${AZURE_CLIENT_ID} -p ${AZURE_CLIENT_SECRET} ${acrSettings.loginServer}"
            container.push()
        }

        stage('Deploy') {
            echo 'Orchestrating a new deployment with kubectl is a simple exercise left to the reader ;)'
        }
    }

}
