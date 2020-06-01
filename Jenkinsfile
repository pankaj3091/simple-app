pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages{
        stage('Build'){
            steps{
                 bat 'mvn clean package deploy'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
		    echo "Build process completed...!!"
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                     nexusArtifactUploader artifacts: [
				[
					artifactId: 'simple-app', 
					classifier: '', 
					file: 'target/simple-app-3.0.0-SNAPSHOT.war', 
					type: 'war'
				]
			], 
				credentialsId: 'nexus_id', 
				groupId: 'in.javahome', 
				nexusUrl: 'localhost:8081/', 
				nexusVersion: 'nexus2', 
				protocol: 'http', 
				repository: 'http://localhost:8081/repository/SAMPLE-SNAP/', 
				version: '3.0.0-SNAPSHOT'
                    }
            }
        }
    }
}
