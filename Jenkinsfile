pipeline {
    agent any
    stages{
        stage('build project'){
            steps{
                git url:'https://github.com/Kiran-Komroju/FinanceME/', branch: "master"
                sh 'mvn clean package'
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t kirankomroju/financeme:1 .'
                    sh 'docker images'
                }
            }
        }
         
       stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push kirankomroju/financeme:1'
                }
            }
        } 
     stage('Deploy') {
            steps {
                ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: '/var/lib/jenkins/workspace/FinanceMe/ansible-playbook.yml', vaultTmpPath: ''
				echo'We have successfully deployed the FinanceMe application using Ansible'
                  
                }
            }
        
    }
}
