pipeline {
    agent any

    parameters {
        string(name: 'DEPLOY_VERSION', defaultValue: 'v1.0.0', description: 'Version for this deployment')
        string(name: 'BRANCH_NAME', defaultValue: 'dev', description: 'Git branch to deploy')
        choice(name: 'ENV', choices: ['dev', 'staging', 'prod'], description: 'Target environment')
    }

   /* environment {
        SSH_KEY_ID = 'ec2-ssh-key' // Jenkins SSH credential ID
        REMOTE_USER = 'ubuntu'
        REMOTE_HOST = '51.20.181.213'
        REMOTE_DIR = '/home/ubuntu/my_app'
        PORT = '3000'
    }*/

    tools {
    nodejs 'node-version' // Match the name you configured
}

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "${params.BRANCH_NAME}"]],
                    userRemoteConfigs: [[
                        url: 'https://github.com/anshulika123/static-html-ec2-deploy.git'
                    ]]
                ])
            }
        }

        stage('Build') {
            steps {
                echo "In Build Stage"
                bat '''
                    npm install
                    "echo After Install"
                    npm run build
                    "echo After build"
                '''
            }
        }

    }

    post {}
}