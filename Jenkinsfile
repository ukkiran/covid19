node {
   stage('init') {
      checkout scm
    }
    stage('build') {
       sh '''
          mvn clean package
        '''
    }

             stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    // Optionally use a Maven environment you've configured already
                  
                        sh 'mvn clean package sonar:sonar'
                    
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }

     }
 


