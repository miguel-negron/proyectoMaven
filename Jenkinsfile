pipeline {
    
    agent any
    
    stages {
        stage("Compilación") {
            steps {
                sh "mvn compile"
            }
        }
        stage("Pruebas") {
            stages {
                stage("Pruebas Dinámicas") {
                    /* Solucion con MAGIA... evitar                    
                    steps {
                        // Compilar pruebas -> Las pruebas no pueden ejecutarse
                        // Ejecutar pruebas -> Genera informe... tanto si se ejecutan bien como si se ejecutan mal
                    }
                    post {
                        always {
                            junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml' 
                        }    
                    }
                    */                    
                    stages {
                        stage("Compilación pruebas") {
                            steps {
                                // Compilar pruebas -> Las pruebas no pueden ejecutarse
                                sh "mvn test-compile"
                            }
                        }
                        stage("Ejecución pruebas") {
                            steps {
                                // Ejecutar pruebas -> Genera informe... tanto si se ejecutan bien como si se ejecutan mal
                                sh "mvn test"
                            }
                            post{
                                always {
                                    junit testResults: 'target/surefire-reports/*.xml' 
                                }    
                            }
                        }
                    }
                }
                stage("SonarQube") {
                    steps {
                        withSonarQubeEnv('sonarqube') {
                            sh """
                        mvn sonar:sonar -Dsonar.projectKey=proyectoMaven \
                        -Dsonar.host.url=http://172.31.11.165:8081 \
                        -Dsonar.login=99ac600688b8f1a2c9ad87607a27882ee9b09df0
                        """
                        }
                        
                        waitForQualityGate false
                    }    
                }
            }
        }
        stage("Empaquetado") {
            steps {
                sh "mvn package -Dmaven.test.skip=true"
            }
            post{
                success {
                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                }
            }
        }
    }
    post {
        always {
            cleanWs deleteDirs: true, patterns: [[pattern: 'target', type: 'INCLUDE']]
        }
    }
}