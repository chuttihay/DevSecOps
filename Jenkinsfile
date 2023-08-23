pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=tannergbuggywebapp -Dsonar.organization=tannerbuggywebapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=0786fa334fde3458f18cb44ec4cb13aeb1361fbc'
			}
        } 
  }

   stage('RunSCAAnalysisUsingSnyk') {
     steps {		
	  withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    
  }
}
