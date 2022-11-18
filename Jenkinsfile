pipeline {
     agent {
        kubernetes {
            defaultContainer 'jnlp'
            yamlFile 'jenkins.yaml'
        }
    }
     environment {
		 dockerhub=credentials('dockerHub')
     }		 

     stages {     
         stage('UPDATE GIT') {
		   steps {	 
               script {
				   catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
					   withCredentials([usernamePassword(credentialsId: 'sandesh-github-pat', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
						   sh "git config user.email sandeshtamboli123@gmail.com"
						   sh "git config user.name sandesh"
						   sh "cat demo/deployment.yaml"
						   sh "sed -i 's+mynamesandesh/jenkins.*+mynamesandesh/jenkins:${env.BUILD_NUMBER}+g' demo/deployment.yaml"
						   sh "cat demo/deployment.yaml"
						   sh "git add demo/deployment.yaml"
						   sh "git commit -m 'Done by jenkins job changemanifest: ${env.BUILD_NUMBER}'"
						   sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/jenkins-argocd.git HEAD:main"
					   }	
					}   
               }
		   }	
		}	
     }
}	 
