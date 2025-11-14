pipeline {
    agent any
    tools {
        jdk 'jdk'
        maven 'maven'
    }
    environment {
        APPLICATION_NAME = 'spring-petclinic'
        BUCKET_NAME = 'chandana21_bucket'
        YOUR_PROJECT_ID = 'leafy-rope-472907-e3'
        BUILD = '/var/lib/jenkins/workspace/usecase2/spring-petclinic'
        UPLOAD_GCS = '/var/lib/jenkins/workspace/usecase2/spring-petclinic/target'
        ARTIFACT = 'spring-petclinic-4.0.0-SNAPSHOT.jar.jar'

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

                    withCredentials([file(credentialsId: 'gcp-sa-key', variable: 'KEY_JSON')]) {
                        sh """
                          set -e
                          gcloud auth activate-service-account 'chandana-artifact-push@leafy-rope-472907-e3.iam.gserviceaccount.com' --key-file=$KEY_JSON
                          gcloud config set project ${YOUR_PROJECT_ID}
                          gsutil cp ${ARTIFACT} gs://${BUCKET_NAME}/artifact/my-app.jar
                        """
                    }
                } 
            } 
        }
    }
}



   
