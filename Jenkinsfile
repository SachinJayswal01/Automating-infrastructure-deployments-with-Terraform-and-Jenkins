pipeline {
    parameters {
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
    } 
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS-Access-key')
        AWS_SECRET_ACCESS_KEY = credentials('AWS-Secret-access-key')
    }
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Plan') {
            steps {
                sh 'terraform init'
                sh "terraform plan"
            }
        }
        stage('Approval') {
           when {
               not {
                   equals expected: true, actual: params.autoApprove
               }
           }

           steps {
               script {
                    input message: "Do you want to apply the plan?",
                    parameters: [text(name: 'Plan', description: 'Please review the plan')]
               }
           }
       }

        stage('Apply') {
            steps {
                sh "terraform apply"
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

