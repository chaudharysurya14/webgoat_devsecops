pipeline {
  agent any 
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                echo "PATH = ${PATH}"
                echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    
    stage ('Check secrets') {
      steps {
      sh 'trufflehog3 https://github.com/chaudharysurya14/webgoat_devsecops.git -f json -o truffelhog_output.json || true'
      }
    }
    
    stage ('Software composition analysis') {
            steps {
                dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'OWASP DP'

                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        } 
	stage ('Static analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh 'mvn sonar:sonar'
        //sh 'sudo python3 Devsecops.py'
	}
      }
    }	  
  }  
}
