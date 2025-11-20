pipeline {
    agent any 
    tools {
        nodejs 'Node_18' 
    }
    environment {
        // Шлях до папки 'cargo' на вашому робочому комп'ютері
        DELIVERY_PATH = '/tmp/cargo_delivery' 
        PROJECT_NAME = 'my-simple-project'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Starting the Build stage...'
                sh 'npm install'
                echo 'Dependencies installed and project built.'
            }
        }

        stage('Test') {
            steps {
                echo 'Starting the Test stage...'
                sh 'npm test' 
                echo 'All tests passed successfully.'
            }
        }

        stage('Delivery') {
            steps {
            echo "Starting the Delivery stage to ${env.DELIVERY_PATH}..."

            sh "tar -czvf ${PROJECT_NAME}.tar.gz --exclude='./.git' --exclude='./node_modules' ."
            
            sh "mkdir -p ${env.DELIVERY_PATH}/${PROJECT_NAME}/"
            
            sh "cp ${PROJECT_NAME}.tar.gz ${env.DELIVERY_PATH}/${PROJECT_NAME}/"
            
            echo "Artifact delivered to: ${env.DELIVERY_PATH}/${PROJECT_NAME}/"
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: '*.tar.gz', fingerprint: true
            echo 'Pipeline finished.'
        }
    }
}
