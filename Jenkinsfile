pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=tannerbuggywebapp -Dsonar.organization=tannerbuggywebapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=220e0960e11041d00f13b34db1d5855ad581e74f'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 def mavenEnvVars = tool name: 'Maven_3_5_2', type: 'hudson.tasks.Maven$MavenInstallation'
                 withEnv(mavenEnvVars) {
                  sh 'mvn clean install'
                 }
                 //app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://227117241347.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
                    app = docker.build{}
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
