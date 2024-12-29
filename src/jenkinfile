pipeline {
    agent any
    tools {
        maven 'maven_3.9.9'
    }
    stages {
        stage('SCM') {
            steps {
                git branch:  'main',
                    url: 'https://github.com/shalu-233/spring-petclinic-dummy.git'
                }
            }
        stage('Build') {
            steps {
                sh 'mvn --version'
                sh 'mvn validate'
            }
        }
    }
}

    