pipeline {

  environment {

    registry = "srikanth26y/webapp"

    registryCredential = 'docker-creds'

    dockerImage = ''

  }

  agent any

  stages {

    stage('Cloning Git') {

      steps {

        git 'https://github.com/srikanth26y/demo-project.git'

      }

    }

    stage('Building image') {

      steps{

        script {

//           dockerImage = docker.build registry + ":$BUILD_NUMBER"
		docker.build("demo-project:${env.BUILD_NUMBER}")

        }

      }

    }

    stage('Deploy Image') {

      steps{

        script {
		echo "Docker Image Tag Name: ${dockerImageTag}"
		
// 		sh "docker stop dockerImage"
		
// 		sh "docker rm dockerImage"
		
		docker.withRegistry( 'https://registry.hub.docker.com',  registryCredential ) {
		 
			dockerImage.push("$env.BUILD_NUMBER}")
		  
			dokcerImage.push("latest")

          }

        }

      }

    }
	stage('Remove Existing Container') {

      steps{
	  script {
			sh '''

			a="$(docker container ls --format="{{.ID}}\t{{.Ports}}" | grep "8000" | awk '{print $1}')"

			echo $a

			if [ -z "$a" ]
			then
			echo "do not delete"
			else
			docker rm -f $a
			fi

			'''
		}
      }

    }
    
    	stage('Run Docker Image in Lab') {

      steps{

        script {

		        sh "docker run -d -p 8000:8000 ${dockerImage.imageName()}"
        }

      }

	}

  }

}
