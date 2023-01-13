pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
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
		withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'password', usernameVariable: 'username')]) {
    		sh 'docker login -u $username -p $password'
		sh 'docker push rufatzakirov/flask-app:$BUILD_ID'
		}
            }
        }
        stage('Deploy') {
            steps {
<<<<<<< HEAD
                sh 'ansible-playbook docker.yaml -u ansible --private-key /home/ansible/.ssh/id_rsa'
=======
                sh 'ansible-playbook docker.yaml -u ansible --become'
>>>>>>> bf50068ddfdd123c7b4d6b30fe5183762bb3738c
            }
        }
    }
}
