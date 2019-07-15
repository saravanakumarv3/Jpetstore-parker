node {
	currentBuild.displayName = "1.${BUILD_NUMBER}"
	def GIT_COMMIT
  stage ('cloning the repository'){
	  
      def scm = git 'https://github.com/saravanakumarv3/Jpetstore-parker.git'
	  GIT_COMMIT = sh(returnStdout: true, script: "git rev-parse HEAD").trim()
	  echo "AAAA ${GIT_COMMIT}"
	  //echo "BBBB ${scm}"
	  //GIT_COMMIT = scm.GIT_COMMIT
	   //echo "**** ${GIT_COMMIT}"
	  
  }
	
 
  stage ('Build') {
      withMaven(jdk: 'JDK_local', maven: 'MVN_Local') {
      sh 'mvn clean package'
	      echo "**** ${GIT_COMMIT}"
	//step($class: 'UploadBuild', tenantId: "5ade13625558f2c6688d15ce", revision: "${GIT_COMMIT}", appName: "JPetStore", requestor: "admin", id: "${newComponentVersionId}" )
	
	     
    }
  }
  
  stage ('Cucumber'){
  withMaven(jdk: 'JDK_local', maven: 'MVN_Local') {
      sh 'mvn test -Dtest=Runner'	     
	  echo("************************** Test Result Upload Started to Velocity****************************")
                        try{
                        step([$class: 'UploadJUnitTestResult',
                            properties: [
                        // Need to change the path of the test result xml result required.               
                                filePath: "target/surefire-reports/TEST-org.mybatis.jpetstore.service.OrderServiceTest.xml",
                                tenant_id: "5ade13625558f2c6688d15ce",
                                appName: "Jpetstore",
                                appExtId: "16914a46-e85a-ada1-33b9-46a466a8e290",
                                name: "Executed in JUnit - ${currentBuild.displayName}",
                                testSetName: "Sample Test Run from Jenkins"]
                           
                        ])}catch(e){
                        throw e
                        }
                       
            echo("************************** Test Result Uploaded Successful to Velocity****************************")
  }           
                


  }
	


}

