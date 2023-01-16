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
               // sh 'ansible-playbook docker.yaml -u ansible --private-key /home/ansible/.ssh/id_rsa --become -i inventory'
            }
        }
    }
	post {
     success { 
        withCredentials([string(credentialsId: 'botToken', variable: 'TOKEN'), string(credentialsId: 'chatID', variable: 'CHAT_ID')]) {
        sh  ("""
            curl -s -X POST https://api.telegram.org/bot${TOKEN}/sendMessage -d chat_id=${CHAT_ID} -d parse_mode=markdown -d text='*${env.JOB_NAME}* : POC *Branch*: ${env.GIT_BRANCH} *Build* : OK *Published* = YES'
        """)
        }
     }

     aborted {
        withCredentials([string(credentialsId: 'botToken', variable: 'TOKEN'), string(credentialsId: 'chatID', variable: 'CHAT_ID')]) {
        sh  ("""
            curl -s -X POST https://api.telegram.org/bot${TOKEN}/sendMessage -d chat_id=${CHAT_ID} -d parse_mode=markdown -d text='*${env.JOB_NAME}* : POC *Branch*: ${env.GIT_BRANCH} *Build* : `Aborted` *Published* = `Aborted`'
        """)
        }
     
     }
     failure {
        withCredentials([string(credentialsId: 'botToken', variable: 'TOKEN'), string(credentialsId: 'chatID', variable: 'CHAT_ID')]) {
        sh  ("""
            curl -s -X POST https://api.telegram.org/bot${TOKEN}/sendMessage -d chat_id=${CHAT_ID} -d parse_mode=markdown -d text='*${env.JOB_NAME}* : POC  *Branch*: ${env.GIT_BRANCH} *Build* : `not OK` *Published* = `no`'
        """)
        }
     }

 }
}
