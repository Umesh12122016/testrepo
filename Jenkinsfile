node { 
    # variable declaration     
	def mvnHome
  
        stage('scm') {
           
                // Get some code from a GitHub repository
               git branch: 'dev', credentialsId: 'git-credentials', url: 'https://github.com/Umesh12122016/maven-calculationApp.git'
               mvnHome = tool 'maven 3.8.1'
           
        }  
        
       
         stage('Clean') {
             
                // Run Maven on a Unix agent.
                sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore=true clean"
             
        }
        
        stage('Package') {
             
                // Run Maven on a Unix agent.
                sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore=true package"
               
        }
        
         stage('Artifact') {
          
                // maven artifacts
               archiveArtifacts 'target/*.war'
              // archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
           
        }
         stage('Connection') {
           
               withCredentials([sshUserPrivateKey(credentialsId: 'deployment-tomcat', keyFileVariable: 'sshKey', passphraseVariable: 'sshPass', usernameVariable: 'sshUser')]) {
               echo 'Successfully Connected our tomcat server instance......'
          
            
        }
        }
        
        stage('Deploy') {
          
                withCredentials([usernameColonPassword(credentialsId: 'linux_tomcat_credentials', variable: 'linux_tomcat_credentials')]) {
                sh "curl -v -u ${linux_tomcat_credentials} -T /var/lib/jenkins/workspace/pipelineproject/target/CalculationMavenApp.war 'http://ec2-35-88-255-15.us-west-2.compute.amazonaws.com:8080/manager/text/deploy?path=/CalculationAppScipted&update=true'"
                }
              
             
            
        }
        
    
}
