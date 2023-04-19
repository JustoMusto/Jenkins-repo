pipeline {
    agent {
        label 'my-agent'
    }
    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['Dev', 'Prod'],
            description: 'The environment to deploy to'
        )
    }

    stages {
        stage('Deploy Nginx Configuration') {
            when {
                expression { params.ENVIRONMENT == 'Dev' || params.ENVIRONMENT == 'Prod' }
            }
            steps {
                script {
                    def config_branch
                    def lb_host

                    if (params.ENVIRONMENT == 'Dev') {
                        config_branch = 'dev'
                        lb_host = '20.232.207.120'
                    } else if (params.ENVIRONMENT == 'Prod') {
                        config_branch = 'prod'
                        lb_host = 'prod-load-balancer-host'
                    }

                    // Clone Nginx config repository
                    git branch: config_branch, url: 'https://github.com/JustoMusto/nginx-repo.git'

                    // Copy Nginx config files to remote host
                    sshagent(['dev-credentials']) {
                        sh "scp -r nginx.conf ${lb_host}:~/nginx"
                    }

                    // Verify Nginx configuration on remote host
                    sshagent(['dev-credentials']) {
                        sh "ssh ${lb_host} 'sudo nginx -t'"
                    }

                    // Create Pull Request for Prod branch if deploying to Dev environment
                    //if (params.ENVIRONMENT == 'Dev') {
                        // TODO: Add code to create Pull Request on Github
                    }
                }
            }
        }
    }
}
