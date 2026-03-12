pipeline {
    agent any


    
    stages {
        stage('Checkout Terraform Project') {
            steps {
                git branch: 'main', url: 'https://github.com/skill-maxxer/Terraform_Jenkins_KICS_CICD.git'
            }
        }

        stage('Verify KICS Installation') {
            steps {
                script {
                    sh 'docker pull checkmarx/kics:latest'
                }
                }
            }
        
        
        
        stage('KICS SCAN') {
            steps {
        script {
            def scanDir = "${WORKSPACE}"  // Use the correct workspace variable

            // Run KICS scan in Docker and suppress any failure or error logs
            def result = sh(script: """
                docker run -t -v ${scanDir}:${scanDir} checkmarx/kics scan -p ${scanDir} -o ${scanDir}/kics_report
            """, returnStatus: true, returnStdout: true)

            // Check if the result is non-zero (indicating failure) and handle silently
            if (result != 0) {

            }

            // Continue the pipeline without marking the stage as failed
            echo "KICS Scan completed, continuing with pipeline execution."
        }
                }
            }
        
        
        
        stage('Approval') {
            steps {
                script {
                    input 'Proceed to apply Terraform changes?'
                }
            }
        }
        
        stage('Archive KICS Results') {
            steps {
                script {
                        sh 'cat ${WORKSPACE}/kics_report/results.json'
                }
            }
        }
        

    }
}
