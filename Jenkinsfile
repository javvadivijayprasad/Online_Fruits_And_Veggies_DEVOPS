 def Build_pass=true
 def Deploy_to_Dev_pass=true
  def UnitTest_pass=true
   def Deploy_to_QA_pass=true
   def E2e_tests_pass= true
pipeline {
agent any
  
stages {
         
stage('Build') {
            steps {
               script{
        try{
            
               if (fileExists('E:/Application/Build.zip')) {
                 bat '''cd /
                cd E:/Application
                del /f Build.zip'''
                } else {
                  echo 'No Build.zip Found'
                }
                checkout([$class: 'GitSCM',  poll: true, branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/javvadivijayprasad/Online_Fruits_And_Veggies_DEVOPS.git']]])
				fileOperations([fileZipOperation(folderPath: '', outputFolderPath: 'E:/Application/'), fileRenameOperation(destination: 'E:/Application/Build.zip', source: 'E:/Application/CI_CD_Pipeline_main.zip')]) 
        }
        catch (Exception e){
        Build_pass = false
    }
                
           
        }
           
               
                }
                
        }      
        
        stage('deploy_to_Dev'){
            
             steps {
                 script
                 {
                 
               if(Build_pass){
                 
              bat '''scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i "C:/Users/Adroitent/Desktop/Devops.pem" E:/Application/Build.zip ec2-user@ec2-65-0-72-161.ap-south-1.compute.amazonaws.com:/home/ec2-user/EAPP
ssh -o "StrictHostKeyChecking no" -i "C:/Users/Dell/Desktop/Devops.pem" ec2-user@ec2-65-0-72-161.ap-south-1.compute.amazonaws.com "cd EAPP; sh deployment.sh" 
'''

}
}
}
}

stage('UnitTest'){

    steps {
     
     script{
	 
	 if(Deploy_to_Dev_pass){
	 
	 try{
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/javvadivijayprasad/UnitTests_Devops.git']]])
		bat 'mvn clean test'
		}
		catch(Exception e){
		UnitTest_pass=false
     }
     
}
	 
    }
}



}

stage('Deploy_to_QA'){
   steps {
                 script
                 {
                 
               if(UnitTest_pass){
                 
				 try{
				 
              bat '''scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i "C:/Users/Adroitent/Desktop/Devops.pem" E:/Application/Build.zip ec2-user@ec2-52-66-54-73.ap-south-1.compute.amazonaws.com:/home/ec2-user/EAPP
ssh -o "StrictHostKeyChecking no" -i "C:/Users/Adroitent/Desktop/Devops.pem" ec2-user@ec2-52-66-54-73.ap-south-1.compute.amazonaws.com "cd EAPP; sh deployment.sh" 
'''

}
catch(Exception e){
Deploy_to_QA_pass=false
}

}
}
}


}

stage('E2e_tests'){
 steps {
 script{
 
 if(Deploy_to_QA_pass){
 
 try{
 
 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/javvadivijayprasad/E2E_tests_Devops.git']]])
bat 'mvn clean test -DsuiteXmlFile=testng.xml'

}
catch(Exception e){
E2e_tests_pass=false
}

}

}
}
}

stage('Release'){
steps {
                 script
                 {
                 
               if(E2e_tests_pass){
			   
                 
              bat '''scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i "C:/Users/Adroitent/Desktop/Devops.pem" E:/Application/Build.zip ec2-user@ec2-35-154-205-94.ap-south-1.compute.amazonaws.com:/home/ec2-user/EAPP
ssh -o "StrictHostKeyChecking no" -i "C:/Users/Adroitent/Desktop/Devops.pem" ec2-user@ec2-35-154-205-94.ap-south-1.compute.amazonaws.com "cd EAPP; sh deployment.sh"
'''
}
}
}
}
}
}
