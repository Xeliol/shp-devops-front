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
    }
}
