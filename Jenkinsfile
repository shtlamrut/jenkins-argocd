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
					   withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
						   sh "git config user.email shtlamrut@gmail.com"
						   sh "git config user.name shtlamrut"
						   sh "cat demo/deployment.yaml"
						   sh "sed -i 's+shtlamrut/jenkins.*+shtlamrut/jenkins:${DOCKERTAG}+g' demo/deployment.yaml"
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
