pipeline {
    agent any
    tools{
        jdk 'jdk17'
        maven 'maven3'
    }
    enviornment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/AryanAtel1034/Project.git'
                }
            }
        }
        stage('Code Compile') {
            steps{
                
               
                    sh "mvn compile"

               
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('File System Scan (Trivy)') {

            // TRIVY Code To install on jenkins 
            /* sudo apt-get install wget apt-transport-https gnupg lsb-release
            wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
            echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
            sudo apt-get update
            sudo apt-get install trivy */
             
                
                steps{
                    sh "trivy fs --format table -o trivy-fs-report.html ."

                }
            
        }
        stage('Sonaqube Analysis ')  {
            // We have to configure sonar cube server on timestamp 1:08:15 in the video  
            steps{
                
                script{
                    
                    withSonarQubeEnv(sonar) {
                        
                        sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Project -Dsonar.projectKey=Project \
                                -Dsonar.java.binaries=. '''
                    }
                }
                    
            }
        }
        
        stage('Quality Gate') {
            //  1:15:08 Webhook Integration must done 
            steps{
                
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'

                }
            }
        }
        stage('Build') {
            steps{
             
                sh "mvn package"

            }
        }
        // stage('Publish to Nexus') {
        //     // TODO 1:17:09
        //     steps{
        //         // todo
        //         WithMaven(globalMavenSettingConfig: 'global-setting', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: "", T)
        //         sh "mvm deploy"
        //     }
        // }
        //  TILL 1:21:00

        // stage('compile') {
        //     steps{
                
        //         script{
        //             sh "mvn compile"

        //         }
        //     }
        // }
       
       
    }
}
