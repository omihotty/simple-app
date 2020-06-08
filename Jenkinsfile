pipeline {
    agent any
    tools {
        maven 'maven'
	    jdk 'java'
    }
//    options {
//        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
//    }

    stages{

        stage('scm'){
            steps{
                git 'https://github.com/javahometech/simple-app'
            }
            
        }

        stage('Build'){
            steps{
                 sh 'mvn clean package'
//                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "Jenkinsartivactssnap" : "Jenkinsartifacts"
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'simple-app', 
                            classifier: '', 
                            file: "target/simple-app-${mavenPom.version}.war", 
                            type: 'war'
                        ]
                    ], 
                    credentialsId: 'ac1d7202-b847-4f4a-aed4-6a1c6c3dedb5', 
                    groupId: 'in.javahome', 
                    nexusUrl: '192.168.43.168:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: nexusRepoName, 
                    version: "${mavenPom.version}"
                    }
            }
        }
    }
}
