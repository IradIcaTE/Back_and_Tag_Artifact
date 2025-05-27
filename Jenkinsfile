pipeline {
    agent {label "First"}

    parameters {
        booleanParam(name: 'ENABLE_BACKUP', defaultValue: 'true', description: 'Enable backup before tagging?')
        choice(name: 'TAG_TYPE', choices: ['dev', 'staging', 'prod'], description: 'Choose the tag type')
    }

    environment {
        BUILD_ID = "build-${TAG-TYPE}-${new Date().format('yyyyMMdd-HHmmss)}"
        ARTIFACTS_DIR = "${env.WORKSPACE/artifacts}"
    }

    stages {
        stage('Prepare Artifacts') {
            steps {
                script {
                    sh '''
                        mkdir -p ${ARTIFACTS_DIR}
                        echo "Sample Artifact - $(date)" > ${ARTIFACT_DIR}/artifact.txt
                    '''
                }
            }
        }

        stage ('Backup Artifcats') {
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
            steos {
                echo "Tagging build with: ${BUILD_TAG}"
                sh '''
                    echo "${BUILD_TAG} > ${ARTIFACTS_DIR/tag.txt}
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