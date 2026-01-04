pipeline {
 agent any

 environment{
     IMAGE_NAME = 'divyj0212/springrestapi'
	 PORT_MAPPING = '8081:7000'
 }
 parameters{
    string(name: 'DEPLOY_ENV',defaultValue: 'development',description: 'select the target environment')
 	string(name: 'APP_VERSION', description: 'Provide tag for the docker image')
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
				docker build -t $IMAGE_NAME:'$APP_VERSION' .
				echo "=======Building Image Completed======="
			"""
		}
	}
} // end of stages

} // end of pipeline
