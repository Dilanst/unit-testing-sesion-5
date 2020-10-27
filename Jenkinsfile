pipeline {
    agent any
    stages {
        stage ('Build Backend') {
            
            environment {
                mvnHOME = tool 'MAVEN_HOME'
            }

            steps {

		sh  "${mvnHOME}/bin/mvn clean package -DskipTests=true"	
            }
        }

	stage ('Run Test') {
            
            environment {
                mvnHOME = tool 'MAVEN_HOME'
            }

            steps {

		sh  "${mvnHOME}/bin/mvn test"	
            }
        }

	stage ('Sonar Analysts') {
            
            environment {
                scanerHome = tool 'SONAR_SCANER'
            }

            steps {
		withSonarQubeEnv('SONAR_LOCAL') {
			sh  "${scanerHome}/bin/sonar-scanner -e -Dsonar.projectKey=backend-br -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=f8af51bde922b8b4f6c9fd7f80728a1563efc5a3 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/src/test/**,**/entity/**"	
		}

            }
        }

    }

	post {
		always {
			junit allowEmptyResults: true, testsResults: 'target/surefire-reports/*.xml'	
	     	}		
	}

}
