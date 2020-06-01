pipeline {
    agent any
    tools {
        maven 'maven'
    }
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
    }
    stages{
        stage('Build'){
            steps{
                 sh 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "simpleapp-snapshot" : "simpleapp-release"
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
