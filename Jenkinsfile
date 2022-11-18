pipeline {
     agent {
        kubernetes {
            defaultContainer 'jnlp'
            yamlFile 'jenkins.yaml'
        }
    }
     environment {
		 dockerhub=credentials('dockerHub')
		 ENV = {'qa', 'prod'}
     }		 

     stages {     
         stage('UPDATE GIT') {
			when {
                allOf {
                    branch "main"
                    environment(name: "ENV", value: "qa")
                }
            }
		    steps {	 
               script {
				   catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
					   withCredentials([usernamePassword(credentialsId: 'sandesh-github-pat', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
						   sh "git config user.email sandeshtamboli123@gmail.com"
						   sh "git config user.name sandesh"
						   sh "cat qa/deployment.yaml"
						   sh "sed -i 's+mynamesandesh/jenkins.*+mynamesandesh/jenkins:${env.BUILD_NUMBER}+g' qa/deployment.yaml"
						   sh "cat qa/deployment.yaml"
						   sh "git add qa/deployment.yaml"
						   sh "git commit -m 'Done by jenkins job changemanifest: ${env.BUILD_NUMBER}'"
						   sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/jenkins-argocd.git HEAD:main"
					   }	
					}   
               }
		   }	
		}	
     }
}	 
