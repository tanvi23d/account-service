node{
    
    def MAVEN_HOME = tool "testmaven"
    env.PATH = "${env.PATH}:${MAVEN_HOME}/bin"

    stage('checkout'){
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/tanvi23d/account-service.git']]])
    }
    
    stage('compile'){
        sh 'mvn clean compile'
    }
    
    stage('unit testing'){
        sh 'mvn test'
    }
    
    stage('Code quality analysis') 
	{
		withSonarQubeEnv('sonarqube') 
		{
                 sh 'mvn sonar:sonar -Dsonar.organization=tanvi23d -Dsonar.projectKey=account-service1'
		
    		}
	 }
	 
	 stage("Quality Gate"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }


}
