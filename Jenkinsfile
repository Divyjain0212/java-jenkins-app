pipeline {
 agent any
 stages{
   stage("checkout"){
     steps {
           sh """
           echo "Checkout done - $PWD"
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
