pipeline {
    agent { label 'slave1' }

    environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerloginid')
    }

    stages {
        stage('SCM_Checkout') {
            steps {
                echo 'Perform SCM Checkout'
                git 'https://github.com/SA-AWS-DevOps-July24/BankingApp.git'
            }
        }

        stage('Application Build') {
            steps {
                echo 'Perform Application Build'
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Perform Docker Build'
                sh "docker build -t akshay14032003/bankingapp:${BUILD_NUMBER} ."
                sh 'docker image list'
             } 
        }

        stage('Login to DockerHub') {
            steps {
                echo 'Login to DockerHub'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Publish the Image to DockerHub') {
            steps {
                echo 'Publish to DockerHub'
                sh "docker push akshay14032003/bankingapp:${BUILD_NUMBER}"
            }
        }


        stage('Deploy with Ansible') {
            steps {
                ansiblePlaybook credentialsId: 'slave1', installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: '/etc/ansible/playbooks/deploy.yaml', vaultTmpPath: ''
               }
           }
       
       }
   }
