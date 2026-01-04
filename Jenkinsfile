pipeline {
 agent any

 environment{
     IMAGE_NAME = 'divyj0212/springrestapi'
	 PORT_MAPPING = '8081:7000'
	 DOCKER_CREDENTIALS = credentials("dockerhub") //[DOCKERHUB_CREDENTIALS_USR,DOCKERHUB_CREDENTIALS_PSW]
 }
 parameters{
    string(name: 'DEPLOY_ENV',defaultValue: 'development',description: 'select the target environment')
 	// string(name: 'APP_VERSION', description: 'Provide tag for the docker image')
 }
 stages{
   stage("checkout"){
	 when {
        expression { 
            return params.DEPLOY_ENV == 'development'
        }
	 }
     steps {
           sh """
           echo "Checkout done - $PWD"
	       echo "DEPLOYMENT ENV SELECTED - $DEPLOY_ENV"
           ls -l
		   echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL} from ${env.NODE_NAME}"
           """
      }
   }

 
   stage("Building the application"){
     steps {
         
        sh 'echo "========Building Java Application============"'
        sh '/opt/apache-maven-3.9.12/bin/mvn -v'
        sh '/opt/apache-maven-3.9.12/bin/mvn clean package -B -DskipTests'
        sh 'echo "======Building Java Application completed====="'
    
      }
    }


	 stage("Testing the application"){
     steps {
        sh 'echo "========Testing Java Application============"'
        sh '/opt/apache-maven-3.9.12/bin/mvn -v'
        sh '/opt/apache-maven-3.9.12/bin/mvn test'
        sh 'echo "======Testing Java Application completed====="'
      }
    }

	stage("Docker image"){
		steps{
			sh """
				echo "=======Build the Docker image========"
				echo "IMAGE Name is - ${IMAGE_NAME}"
				docker build -t $IMAGE_NAME:"${env.BUILD_NUMBER}" .
				echo "=======Building Image Completed======="
			"""
		}
	}

	stage("Scan the Image"){
		 steps{
			 sh 'echo "=======Scanning Image Started========"'
			 //trivy image $IMAGE_NAME:"${env.BUILD_NUMBER}"
			 sh"""
			 echo "========Scanning Completed============"
			 """
		 }
	}
			 
	stage("Docker Login and Push the image"){
		steps{
			sh """
			echo "======Login the Docker Hub======"
			echo "Docker Credentials:- ${DOCKER_CREDENTIALS}"
			docker login -u $DOCKER_CREDENTIALS_USR -p $DOCKER_CREDENTIALS_PSW
			docker push $IMAGE_NAME:${env.BUILD_NUMBER} 
			echo "======Login and Push Image Successful======="
			"""
		}
	}
} // end of stages

} // end of pipeline
