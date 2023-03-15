pipeline {
    environment{
        imageName =""
    }
    agent any
    stages {
        stage('Git Pull') {
            steps {
                git 'https://github.com/akashanand842/MiniProjectSPE.git'
            }
        }
        stage('Build') {
            steps {
                script{
                    sh 'mvn clean install'
                }
            }
        }
        stage('Building an Docker Image') {
            steps {
                script{
                    imageName=docker.build "akashanand842/spe_mini_project"
                }
            }
        }

        stage('Push The Docker Image') {
            steps {
                script{
                    docker.withRegistry('','docker-key'){
                        imageName.push()
                    }
                }
            }
        }
        stage('Ansible Pull Docker Image') {
            steps {
                ansiblePlaybook becomeUser: null, colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'ansible-deploy/inventory',
                 playbook: 'ansible-deploy/ansible-book.yml', sudoUser: null

            }
        }
    }
}
