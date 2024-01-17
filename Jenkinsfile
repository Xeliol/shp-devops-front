pipeline {
    agent none
    environment {
        IMAGE_NAME = 'xeliol/django_demo'
        HUB_CRED_ID = 'Docker_Marinin'
        PROJECT_DIR = 'common_marinin_django'
    }
    stages {
        stage("deps") {
            agent {
                docker {
                    image 'node:latest'
                    args '-u root -v ${WORKSPACE}/pipenv:/root/.local'
                }
            }
            steps {
                sh 'npm install'
            }
        }
        stage("build") {
            agent {
                docker {
                    image 'node:latest'
                    args '-u root -v ${WORKSPACE}/pipenv:/root/.local'
                }
            }
            steps {
                sh 'npm run build_prod'
            }
        }
         stage("deploy") {
        	agent any
        	steps {
        		withCredentials(
        			[
        				string(credentialsId: "production_ip", variable: "SERVER_IP"),
        				sshUserPrivateKey(credentialsId: "production_key", keyFileVariable: "SERVER_KEY", usernameVariable: "SERVER_USERNAME")
        			]
        		) 
        		{
        			sh 'ssh -i ${SERVER_KEY} ${SERVER_USERNAME}@${SERVER_IP} mkdir -p /var/www/marinin_random_numbers'
                    sh 'scp -i ${SERVER_KEY} -r dist/* ${SERVER_USERNAME}@${SERVER_IP}:/var/www/marinin_random_numbers'			
        		}
        	}
        }
    }
}
