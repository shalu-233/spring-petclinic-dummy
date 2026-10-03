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
                git branch:  "${branch}",
                    url: "${url}"
                }
            }
        stage('Build') {
            steps {
                sh 'mvn --version'
                sh 'mvn validate'
                sh 'mvn clean package'
            }
        }
    }
}

    