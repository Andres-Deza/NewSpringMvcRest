pipeline {
    agent any
        stages {
        stage('Initialize'){
            steps{
                echo "Esta es el inicio"
                 slackSend (color: '#36a64f', message: "¡Estamos iniciando :tada:",
                           channel: '#fundamentos-de-devops', tokenCredentialId: 'secretTextSlack')
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B package'
            slackSend (color: '#36a64f', message: "¡Comenzamo el build :tada:",
                           channel: '#fundamentos-de-devops', tokenCredentialId: 'secretTextSlack')
            }
        }
            
        stage('Test') {
            steps {
                 sh "mvn clean verify" 
                slackSend (color: '#36a64f', message: "¡Testeando :tada:",
                           channel: '#fundamentos-de-devops', tokenCredentialId: 'secretTextSlack')
            
            }
        } 
        
      stage('SonarQube analysis') {
          steps {
            script {
              def scannerHome = tool 'SonarQubeConexion'
              withSonarQubeEnv('SonarQube') {
                sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=PruebaPipelines -Dsonar.sources=src -Dsonar.java.binaries=target/classes -Dsonar.host.url=http://192.168.1.192:9000 -Dsonar.login=sqp_ac73c8ce8270b65a4b9691a6453e5508e6a192f4"
              }
            }
          }
        }
        
        
        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: "nexus3",
                            protocol: "http",
                            nexusUrl: "192.168.1.192:8082",
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: "TestMavenHost",
                            credentialsId: "nexus_auth",
                            artifacts: [
                                [artifactId: pom.artifactId,
                                        classifier: '',
                                        file: artifactPath,
                                        type: pom.packaging]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
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
