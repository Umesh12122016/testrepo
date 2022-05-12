node { 
    # variable declaration     
	def mvnHome
  
        stage('scm') {
           
                // Get some code from a GitHub repository
               git branch: 'dev', credentialsId: 'git-credentials', url: 'https://github.com/markondareddy/maven-calculationApp.git'
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
          
                withCredentials([usernameColonPassword(credentialsId: 'ubuntu_tomcat_credentials', variable: 'ubuntu_tomcat_credentials')]) {
                sh "curl -v -u ${ubuntu_tomcat_credentials} -T /var/lib/jenkins/workspace/pipelineproject/target/CalculationMavenApp.war 'http://ec2-3-80-164-102.compute-1.amazonaws.com:8080/manager/text/deploy?path=/CalculationAppScipted&update=true'"
                }
              
             
            
        }
        
    
}
