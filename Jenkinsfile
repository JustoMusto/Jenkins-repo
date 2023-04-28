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

                    
                    }
                }
            }
            stage('Dev deployment') {
                 if (params.ENVIRONMENT == 'Dev') {
                    // Login to azure VM, scp nginx.conf, and test new configuration
                    sshagent(credentials: ['dev-credentials']) {
                        sh "ssh  ${lb_host}"
                        sh "scp -r nginx.conf ${lb_host}:/etc/nginx/nginx.conf"
                        sh "ssh  ${lb_host} 'sudo nginx -t'"
                        
                    }
                 }
            }
        }
    }
