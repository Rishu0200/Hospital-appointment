pipeline{
    agent any

    environment {
        VENV_DIR = 'venv'
        GCP_PROJECT = "hospital-appointment-479317"
        GCLOUD_PATH = "/var/jenkins_home/google-cloud-sdk/bin"
    }
    
    stages{
        stage('Cloning Github repo to Jenkins'){
            steps{
                script{
                    echo 'Cloning Github repo to Jenkins............'
                    checkout scmGit(
                        branches: [[name: '*/main']], 
                        extensions: [], 
                        userRemoteConfigs: [[
                            credentialsId: 'Hospital-github-token', 
                            url: 'https://github.com/Rishu0200/Hospital-appointment.git'
                        ]]
                    )
                }
            }  
        }

        stage('Setting up our Virtual Environment and Installing dependancies'){
            steps{
                script{
                    echo 'Setting up our Virtual Environment and Installing dependancies............'
                    sh '''
                        python -m venv ${VENV_DIR}
                        . ${VENV_DIR}/bin/activate
                        pip install --upgrade pip
                        pip install -e .
                    '''
                }
            }
        }

        stage('Building and Pushing Docker Image to GCR'){
            steps{
                withCredentials([file(credentialsId: 'HOSPITAL-GCP-KEY', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]){
                    script{
                        echo 'Building and Pushing Docker Image to GCR.............'
                        sh '''
                            set -e  # Exit on any error
                            
                            export PATH=$PATH:${GCLOUD_PATH}
                            
                            echo "Authenticating with GCP..."
                            gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}
                            
                            echo "Setting GCP project..."
                            gcloud config set project ${GCP_PROJECT}
                            
                            echo "Configuring Docker for GCR..."
                            gcloud auth configure-docker gcr.io --quiet
                            
                            echo "Building Docker image..."
                            docker build -t gcr.io/${GCP_PROJECT}/ml-project:latest .
                            
                            echo "Pushing Docker image to GCR..."
                            docker push gcr.io/${GCP_PROJECT}/ml-project:latest
                            
                            echo "Image pushed successfully!"
                        '''
                    }
                }
            }
        }

        stage('Deploy to Google Cloud Run'){
            steps{
                withCredentials([file(credentialsId: 'HOSPITAL-GCP-KEY', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]){  
                    script{
                        echo 'Deploy to Google Cloud Run.............'
                        sh '''
                            export PATH=$PATH:${GCLOUD_PATH}

                            gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}
                            gcloud config set project ${GCP_PROJECT}

                            gcloud run deploy ml-project \
                                --image=gcr.io/${GCP_PROJECT}/ml-project:latest \
                                --platform=managed \
                                --region=us-central1 \
                                --allow-unauthenticated
                        '''
                    }
                }
            }
        }
    }        
}


