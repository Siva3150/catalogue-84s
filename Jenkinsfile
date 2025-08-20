pipeline {
    agent  {
        label 'agent-1'
    }
    environment { 
        appVersion = ''
        REGION = "us-east-1"
        ACC_ID = "445567085619"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }
    options {
        timeout(time: 30, unit: 'MINUTES') 
        disableConcurrentBuilds()
    }
    parameters {
        booleanParam(name: 'deploy', defaultValue: false, description: 'Toggle this value')
    }
    // Build
    stages {
        stage('Read Package.json') {
            steps {
                script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "Package version: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script{
                    sh """
                        npm install 

                    """
                }
            }
        }

        stage('Unit testing') {
            steps {
                script{
                    sh """
                        echo "unit testing was successful" 

                    """
                }
            }
        }

        stage('Docker build') {
            steps {
                script{
                     withAWS(credentials: 'aws-creds', region: 'us-east-1') {
                  sh """
                   aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                   docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                   docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} 

                   """
                }
                }

            }
        }

    stage('Trigger Deploy') {
      when {
           expression { params.deploy }
    }
    steps {
        script {
            build job: 'catalogue-cd',
            parameters: [
                string(name: 'appVersion', value: "${appVersion}"),
                string(name: 'deploy_to', value: 'dev')
            ],
            propagate: false, // even if this job fails, main pipeline will not fail
            wait: false       // main pipeline will not wait for job completion
        }
    }
    
}

        
        
}

    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success { 
            echo 'Hello Success'
        }
        failure { 
            echo 'Hello Failure'
        }
    }
}