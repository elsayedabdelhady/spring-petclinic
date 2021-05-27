pipeline {
agent any
    environment {
    tomcat_url = '192.168.1.3:9090'
    project_name = '/PetElsayed'
    }
    stages {
    stage('SCM Checkout') {
        steps {
        echo '>>> Start getting SCM code'
        git branch: 'main', url: 'https://github.com/elsayedabdelhady/spring-petclinic.git/'
        echo '>>> End getting SCM code'
        }
}
    stage('Build') {
        steps {
        echo '>>> Start Build using Maven'
        sh 'mvn -B -DskipTests clean package'
        echo '>>> end Build using Maven'
        }
    }
    stage('test') {
        steps {
        echo '>>> Start testing using Maven'
        sh "mvn test"
        echo '>>> end testing using Maven'
        }
    }
    stage('Sanity Check') {
        steps {
        sh '''#!/bin/bash
        url="${tomcat_url}${project_name}"
        read -ra result <<< $(curl -Is --connect-timeout 5 "${url}" || echo "timeout 500")
        status=${result[1]}
        [ $status -eq 200 ] && echo " $url with status $status is >>> Running"
        [ $status -ge 400 ] && echo "bounce at $url with status >>>>> $status"
        '''
        }
    }
}
}
