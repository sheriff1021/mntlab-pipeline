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
                sh '/opt/gradle/gradle-7.0/bin/gradle build'
            }            
        }
    }
}
