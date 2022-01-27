pipeline {
    
    agent any
    
    stages {
        stage("Etapa 1") {
            steps {
                sh "echo soy la etapa 1"
                
            }
            
            post {
                always {
                    sh "echo acabó"
                }
                
                success {
                    sh "echo acabó bien"
                }
                
                failure {
                    sh "echo acabó mal"
                }
                
            }
        }
        stage("Etapa 2") {
            stages {
                stage("Etapa 2 anidada") {
            steps {
                sh "echo soy la etapa 2 anidada"
                
            }
            
            post {
                always {
                    sh "echo acabó"
                }
                
                success {
                    sh "echo acabó bien"
                }
                
                failure {
                    sh "echo acabó mal"
                }
                
            }
        }
            }
        }
    }
}