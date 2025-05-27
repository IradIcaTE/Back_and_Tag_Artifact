pipeline {
    agent any

    parameters {
        booleanParam(name: 'ENABLE_BACKUP', defaultValue: true, description: 'Enable backup before tagging?')
        choice(name: 'TAG_TYPE', choices: ['dev', 'staging', 'prod'], description: 'Choose the tag type')
    }

    environment {
        BUILD_TAG = "build-${TAG_TYPE}-${new Date().format('yyyyMMdd-HHmmss')}"
        ARTIFACTS_DIR = "${env.WORKSPACE}/artifacts"
    }

    stages {
        stage('Prepare Artifacts') {
            steps {
                script {
                    sh '''
                        mkdir -p ${ARTIFACTS_DIR}
                        echo "Sample Artifact - $(date)" > ${ARTIFACTS_DIR}/artifact.txt
                    '''
                }
            }
        }

        stage('Backup Artifacts') {
            when {
                expression { params.ENABLE_BACKUP }
            }
            steps {
                echo "Backing up artifacts..."
                sh '''
                    mkdir -p ${ARTIFACTS_DIR}/backup
                    cp ${ARTIFACTS_DIR}/artifact.txt ${ARTIFACTS_DIR}/backup/artifact_backup.txt
                '''
            }
        }

        stage('Tag Build') {
            steps {
                echo "Tagging build with: ${BUILD_TAG}"
                sh '''
                    echo "${BUILD_TAG}" > ${ARTIFACTS_DIR}/tag.txt
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'artifacts/**', fingerprint: true
        }
    }
}
