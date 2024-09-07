pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES') 
        disableConcurrentBuilds()
        ansiColor('xterm') //plugin for colors
    }
    parameters { //asking user for choice to apply or destroy
        choice(name: 'action', choices: ['Apply', 'Destroy'], description: 'Pick something')
    }
    stages {
        stage('init') {
            steps {
                sh '''
                cd 01-vpc
                terraform init -reconfigure
                '''
            }
        }
        stage('Plan') {
            when {
                expression {
                    params.action == 'Apply'
                }
            }
            steps {
                sh '''
                cd 01-vpc
                terraform plan
                '''
            }
        }
        stage('Deploy') {
            when {
                expression {
                    params.action == 'Apply'
                }
            }
            input {
                message "Should we Continue ?"
                ok "Yes, we should."
            }
            steps {
                sh '''
                cd 01-vpc
                terraform apply -auto-approve
                '''
            }
        }
        stage('Destroy') {
            when {
                expression {
                    params.action == 'Destroy'
                }
            }
            steps {
                sh '''
                cd 01-vpc
                terraform destroy -auto-approve
                '''
            }
        }
    }
    post {  //executes based on whether if it is success or failure
        always { 
            echo 'I will always say Hello again!'
            deleteDir() //this will delete the directory created for every build which is recommended.
        }
        success { 
            echo 'I will run when pipeline is success'
        }
        failure { 
            echo 'I will run when pipeline is failure'
        }
    }
}
