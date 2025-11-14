pipeline {
    agent any
    tools {
        jdk 'jdk'
        maven 'maven'
    }
    environment {
        APPLICATION_NAME = 'spring-petclinic'
        BUCKET_NAME = 'venkat-473005'
        YOUR_PROJECT_ID = 'venkat-473005'
        BUILD = '/var/lib/jenkins/workspace/nag-first/spring-petclinic'
        UPLOAD_GCS = '/var/lib/jenkins/workspace/nag-first/spring-petclinic/target'
        ARTIFACT = 'spring-petclinic-4.0.0-SNAPSHOT.jar'

    }
    stages {
        stage('checkout') {
            steps{
                cleanWs()
                sh "git clone https://github.com/spring-projects/spring-petclinic.git"
            }
        }
        stage('build') {
            steps{
                dir("${BUILD}") {
                    echo "Building ${APPLICATION_NAME}"
                    sh "mvn clean package -DskipTests=true"
                }
            }
        }
        stage('UploadtoGCS') {
            steps{
                dir("${UPLOAD_GCS}") {
                    echo "Upload to my GCS Bucket"

                    withCredentials([file(credentialsId: 'gcp-service-account', variable: 'GCP_KEY')]) {
                        sh """
                          set -e
                          gcloud auth activate-service-account 'demo-sesh@venkat-473005.iam.gserviceaccount.com' --key-file=$GCP_KEY
                          gcloud config set project ${YOUR_PROJECT_ID}
                          gsutil cp ${ARTIFACT} gs://${BUCKET_NAME}/artifact/my-app.jar
                        """
                    }
                } 
            } 
        }
    }
}



   
