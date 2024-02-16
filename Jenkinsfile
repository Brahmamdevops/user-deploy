pipeline {
    agent {
        node {
            label 'AGENT-1'  
        }
    }
    
    //  environment { 
    //     GREETING = 'Hello Jenkins'
    // }

    options {
        ansiColor('xterm')
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
    }
        parameters {
        string(name: 'version', defaultValue: '', description: 'Who should I say hello to?')
        string(name: 'environment', defaultValue: '', description: 'Who should I say hello to?')
       
    }
    stages {
        stage('print version') {
            steps {
                sh """
                echo "version : ${params.version}"
                echo "environment : ${params.environment}"
            """
        }
        }

        stage('init') {
            steps {
                sh """
                    cd terraform 
                    terraform init --backend-config=${params.environment}/backend.tf -reconfigure
             """
        }
        }
         stage('plan') {
            steps {
                sh """
                    cd terraform 
                    terraform plan -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}"
             """
        }
        }

        stage('apply') {
            steps {
                sh """
                    cd terraform 
                    terraform apply -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}" -auto-approve
                    
             """
        }
        }
    }
    // post build
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        failure { 
            echo 'this runs when pipeline is failed, used generally to send some alerts'
        }
        success{
            echo 'I will say Hello when pipeline is success'
        }
    }
}
