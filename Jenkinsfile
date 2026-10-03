pipeline {
    agent { label 'springnode'}

    tools {
        maven 'maven3.10.0'
    }
    options {
        timeout ( time: 1, unit: 'HOURS')
    }
    triggers {
        pollSCM ( 'H * * * *' )
    }
    parameters {
        string ( name: 'branch', defaultValue: 'main' )
        string ( name: 'url', defaultValue: 'https://github.com/shalu-233/spring-petclinic-dummy.git' )
    }
    stages {
        stage('SCM') {
            steps {
                git branch:  "${params.branch}",
                    url: "${params.url}"
                }
            }
        stage('Build') {
            steps {
                sh 'mvn --version'
                sh 'mvn validate'
                sh 'mvn clean package'
            }
        }
        stage ('sonarqube analysis') {
            steps {
                withSonarQubeEnv( credentialsId : 'SONAR_ID', installationName :'sonarqube') {
                    // Optionally use a Maven environment you've configured already
                    withMaven(maven:'maven3.10.0') {
                        sh 'mvn clean package org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar -D sonar.projectKey:shalu-233_spring-petclinic-dummy -D sonar.Organization=shalu-233'
                    }
                    junit testResults: '**/surefire-reports/*.xml'
                    archive: 
            }
        }
                        
    }
        stage('Exec Maven commands') {            
            steps {                               
                jf 'mvn-config --repo-resolve-releases shalu-233-libs-release --repo-resolve-snapshots shalu-233-libs-snapshot --repo-deploy-releases shalu-233-libs-release-local --repo-deploy-snapshots shalu-233-libs-snapshot-local'                              
                jf 'mvn clean install'            
            }        
        }        
        stage('Publish build info') {            
            steps {                
                jf 'rt build-publish'            
                }        
            }        
        stage("Quality Gate") {            
            steps {                
                timeout(time: 1, unit: 'HOURS') 
                {                    
                    waitForQualityGate abortPipeline: true}
            }
        }
    }
}
    