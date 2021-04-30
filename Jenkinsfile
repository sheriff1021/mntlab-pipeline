#!groovy
pipeline {
    agent any    

    stages {
        stage('Checkout') {
        	git ([url: 'https://github.com/sheriff1021/mntlab-pipeline.git', branch: 'volodya'])
        }
	stage('Build stage') {
		sh './opt/gradle/gradle-7.0/bin/gradle build'   
	}
    }
}

