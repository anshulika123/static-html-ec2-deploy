pipeline {
    agent any

    parameters {
        string(name: 'DEPLOY_VERSION', defaultValue: 'v1.0.0', description: 'Version for this deployment')
        string(name: 'BRANCH_NAME', defaultValue: 'dev', description: 'Git branch to deploy')
        choice(name: 'ENV', choices: ['dev', 'staging', 'prod'], description: 'Target environment')
    }

    environment {
        SSH_KEY_ID = 'ec2-ssh-key' // Jenkins SSH credential ID
        REMOTE_USER = 'ubuntu'
        REMOTE_HOST = '34.203.206.150'
        REMOTE_DIR = '/home/ubuntu/static-html-ec2-deploy'
        // PORT = '3000'
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

        stage('Deploy') {
            steps {
                echo "Deploy for branch ${params.BRANCH_NAME} -- ENV: ${params.ENV}"
                
                withCredentials([sshUserPrivateKey(credentialsId: "${env.SSH_KEY_ID}", keyFileVariable: 'KEYFILE')]) {
                        bat """
                            echo Using key: %KEYFILE%

                            scp -o StrictHostKeyChecking=no -i %KEYFILE% index.html ${env.REMOTE_USER}@${env.REMOTE_HOST}:${env.REMOTE_DIR}/

                            ssh -o StrictHostKeyChecking=no -i %KEYFILE% ${env.REMOTE_USER}@${env.REMOTE_HOST} "sudo mv ${env.REMOTE_DIR}/index.html /var/www/html/index.html"
                        """
                    }

            }
        }

    }

    post {
        failure {
            echo "Deployment failed. Rolling back..."
        }
    }
}