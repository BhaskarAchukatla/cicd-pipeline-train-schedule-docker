pipeline {
    agent any
    environment {
        JAVA_HOME = '/opt/java/openjdk/bin/java'
    }
    stages {
        stage('Build') {
            steps {
                script {
                    sh './gradlew wrapper --gradle-version=7.0'
                }
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app=docker.build("bhaskardocker1/train-schedule-jenkins") 
                    app.inside { 
                        sh 'echo $(curl localhost:8000)'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}") 
                        app.push("latest")
                    }
                }
            }
        }
    }
}




                
