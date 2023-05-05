pipeline {
    agent any
        stages {
        stage('Initialize'){
            steps{
                echo "Esta es el inicio"
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B package'
            
            }
        }
            
        stage('Test') {
            steps {
                 sh "mvn clean verify" 
            
            }
        } 
        
      
        
            } 
            
            
            
            
            
     }
    
    
post {
        success {
            // Configurar y enviar la notificación en Slack si la construcción es exitosa
            slackSend (color: '#36a64f', message: "¡La construcción ha sido exitosa! :white_check_mark:",
                       channel: '#fundamentos-de-devops', tokenCredentialId: 'key-slack')
        }
        failure {
            // Configurar y enviar la notificación en Slack si la construcción falla
            slackSend (color: '#ff0000', message: "¡La construcción ha fallado! :x:",
                       channel: '#fundamentos-de-devops', tokenCredentialId: 'key-slack')
        }
    }
    
}
