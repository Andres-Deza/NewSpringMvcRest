pipeline {
    agent any
        stages {
        stage('Initialize'){
            steps{
                echo "Esta es el inicio"
                 slackSend (color: '#36a64f', message: "¡Estamos iniciando :tada:",
                           channel: '#canal-de-slack', tokenCredentialId: 'secretTextSlack')
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B package'
            slackSend (color: '#36a64f', message: "¡Comenzamo el build :tada:",
                           channel: '#canal-de-slack', tokenCredentialId: 'secretTextSlack')
            }
        }
            
        stage('Test') {
            steps {
                 sh "mvn clean verify" 
                slackSend (color: '#36a64f', message: "¡Testeando :tada:",
                           channel: '#canal-de-slack', tokenCredentialId: 'secretTextSlack')
            
            }
        } 
        
      
        
            
            
            
            
            
            
     }
    
    
post {
        success {
            // Configurar y enviar la notificación en Slack si la construcción es exitosa
            slackSend (color: '#36a64f', message: "¡La construcción ha sido exitosa! :white_check_mark:",
                       channel: '#fundamentos-de-devops', tokenCredentialId: 'secretTextSlack')
        }
        failure {
            // Configurar y enviar la notificación en Slack si la construcción falla
            slackSend (color: '#ff0000', message: "¡La construcción ha fallado! :x:",
                       channel: '#fundamentos-de-devops', tokenCredentialId: 'secretTextSlack')
        }
    }
    
}
