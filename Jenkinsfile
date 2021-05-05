#!groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps{
                git ([url: 'https://github.com/sheriff1021/mntlab-pipeline.git', branch: 'volodya'])
            }
        }
        stage('Build') {
            steps{
                sh '/opt/gradle/gradle-6.2.2/bin/gradle build'
            }            
        }
	stage('Test') {
	    steps{
	    	 parallel(
			'Unit Tests': {sh '/opt/gradle/gradle-6.2.2/bin/gradle test'},
                	'Jacoco Tests': {sh '/opt/gradle/gradle-6.2.2/bin/gradle jacocoTestReport' },
                	'Cucumber Tests': {sh '/opt/gradle/gradle-6.2.2/bin/gradle cucumber'})
	    }		
	}
 	stage('Trigger') {
		steps{
			build job: "children1-job", parameters: [string(name: 'BRANCH_NAME', value: 'volodya')], wait: true
			step ([$class: 'CopyArtifact',
                	projectName: 'children1-job',
                	filter: 'volodya_dsl_script.tar.gz']);
		}	
	}
	stage('Publish') {
		steps{
			/*sh "tar -xzf volodya_dsl_script.tar.gz jobs.groovy"*/
			sh "tar -czf pipeline-volodya-${BUILD_NUMBER}.tar.gz Jenkinsfile /var/lib/jenkins/workspace/SomePipe/build/libs/gradle-simple.jar"
			sh "curl -v --user 'admin:admin' --upload-file ./pipeline-volodya-${BUILD_NUMBER}.tar.gz http://localhost:8081/repository/pipe/pipeline-volodya-${BUILD_NUMBER}.tar.gz"
		}
	}
	stage('Asking for manual approval') {
		steps {
			timeout(time: 120, unit: 'SECONDS') {
        		input message: 'Do you want to release this build?', ok: 'Yes' 
    			}
		}
	}
	stage('Deployment') {
		steps {
			sh "java -jar build/libs/gradle-simple.jar"
		}
	}
	stage('Sending status') {
		steps{
		sh 'echo "SUCCESS"'
		}
	}
    }
}
