pipeline {
    agent any
    stages {
        stage('compile') {
	   steps {
                echo 'compiling..'
		git 'https://github.com/sahubibh/samplejavaapp.git'
		bat 'bat label: \'C:\\Program Files\\apache-maven-3.8.3\\bin\', script: \'mvn compile\''
		 
           }
        }
        stage('codereview-pmd') {
	   steps {
                echo 'codereview..'
		bat 'bat label: \'C:\\Program Files\\apache-maven-3.8.3\\bin\', script: \'mvn -P metrics pmd:pmd\''
           }
	   post {
               success {
		   recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
               }
           }		
        }
        stage('unit-test') {
	   steps {
                echo 'unittest..'
	        
		bat 'bat label: \'C:\\Program Files\\apache-maven-3.8.3\\bin\', script: \'mvn test\''
                 }
	   post {
               success {
                   junit 'target/surefire-reports/*.xml'
               }
           }			
        }
        stage('codecoverate') {
	   steps {
                echo 'codecoverage..'
		bat 'C:\Program Files\apache-maven-3.8.3\bin\mvn cobertura:cobertura -Dcobertura.report.format=xml'
           }
	   post {
               success {
	           cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false                  
               }
           }		
        }
        stage('package') {
	   steps {
                echo 'package......'
		
		bat 'bat label: \'C:\\Program Files\\apache-maven-3.8.3\\bin\', script: \'mvn package\''
           }		
        }
    }
}
