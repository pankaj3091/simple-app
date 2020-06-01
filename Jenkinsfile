pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages{
        stage('Build'){
            steps{
                 bat 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
		    echo "Build process completed...!!"
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "SAMPLE-SNAP" : "SAMPLE-REL"
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
