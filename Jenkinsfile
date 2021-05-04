#!groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps{
                git ([url: 'https://github.com/sheriff1021/mntlab-pipeline.git', branch: 'volodya'])
            }
        }
        stage('Build stage') {
            steps{
                sh '/opt/gradle/gradle-6.2.2/bin/gradle build'
            }            
        }
	stage('Testing') {
	    steps{
	    	 parallel(
			'Unit Tests': {sh '/opt/gradle/gradle-6.2.2/bin/gradle test'},
                	'Jacoco Tests': {sh '/opt/gradle/bin/gradle-6.2.2/gradle jacocoTestReport' },
                	'Cucumber Tests': {sh '/opt/gradle/gradle-6.2.2/bin/gradle cucumber'})
	    }		
	}
    }
}
