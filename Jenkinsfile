pipeline {
 agent any
 
 parameters{
    string(name: 'DEPLOY_ENV',defaultValue: 'development',description: 'select the target environment')
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
        sh '/opt/apache-maven-3.9.12/bin/mvn clean package -B'
        sh 'echo "======Building Java Application completed====="'
    
      }
    }
} // end of stages

} // end of pipeline
