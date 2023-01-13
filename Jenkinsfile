pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
		sh 'docker build -t rufatzakirov/flask-app:$BUILD_ID .'
            }
        }
        stage('Push') {
            steps {
                echo 'Push to DockerHUB'
		withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'password', usernameVariable: 'username')]) {
    		sh 'docker login -u $username -p $password'
		sh 'docker push rufatzakirov/flask-app:$BUILD_ID'
		}
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
