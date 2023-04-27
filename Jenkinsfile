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
        stage('Prep') {
            when {
                expression { params.ENVIRONMENT == 'Dev' || params.ENVIRONMENT == 'Prod' }
            }
            steps {
                script {
                    def config_branch
                    def lb_host

                    if (params.ENVIRONMENT == 'Dev') {
                        config_branch = 'dev'
                        lb_host = 'azureuser@20.232.207.120'
                    } else if (params.ENVIRONMENT == 'Prod') {
                        config_branch = 'prod'
                        lb_host = 'prod-load-balancer-host'
                    }

                    // Clone Nginx config repository
                    git branch: config_branch, url: 'https://github.com/JustoMusto/nginx-repo.git'

                    // Login to azure VM
                    sshagent(credentials: ['dev-credentials']) {
                        sh "ssh  ${lb_host}"
                        sh 'sudo nginx -t'
                        //sh "ssh ${lb_host} 'scp -r nginx.conf ${lb_host}:/etc/nginx/nginx.conf'"
                    }
                    
                    // Copy Nginx config files to remote host
                    //sshagent(credentials: ['dev-credentials']) {
                    //    sh "scp -r nginx.conf ${lb_host}:/etc/nginx/nginx.conf"
                    //}

                    // Verify Nginx configuration on remote host
                    //sshagent(['dev-credentials']) {
                    //    sh "ssh ${lb_host} 'sudo nginx -t'"
                    //}

                    // Create Pull Request for Prod branch if deploying to Dev environment
                    //if (params.ENVIRONMENT == 'Dev') {
                        // TODO: Add code to create Pull Request on Github
                    }
                }
            }
        }
    }
