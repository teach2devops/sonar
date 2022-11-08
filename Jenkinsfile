pipeline {
   tools {
        maven 'Maven3'
    }
    agent any
      
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/akannan1087/springboot-app']]])     
            }
        }
        
        stage('Quality Gate Statuc Check'){

            steps{
                script{
                withSonarQubeEnv('sonarqube') { 
                sh "mvn sonar:sonar"
                }
                timeout(time: 1, unit: 'HOURS') {
                def qg = waitForQualityGate()
                if (qg.status != 'OK') {
                     error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
                }
		        sh "mvn clean install"
                }
            }  
        }

        
    }
}
