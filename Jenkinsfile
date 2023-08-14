pipeline {
    agent none
    environment {
        REGION="ap-southeast-1"
        FRONTEND_REPO="application-frontend"
        BACKEND_REPO="application-backend"
        REPO_URI="885234002156.dkr.ecr.ap-southeast-1.amazonaws.com"
        IMAGE_TAG="${BUILD_NUMBER}"
    }
    stages {
        stage('Docker Build Frontend') {
            agent any
            steps {
                withAWS(region:'ap-southeast-1',credentials:'aws-credential') {
                    sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${REPO_URI}"
                    sh "docker build -t ${FRONTEND_REPO} frontend/"
                    sh "docker tag ${FRONTEND_REPO}:latest ${REPO_URI}/${FRONTEND_REPO}:${IMAGE_TAG}"
                    sh "docker push ${REPO_URI}/${FRONTEND_REPO}:${IMAGE_TAG}"
                }
            }
        }
        stage('Docker Build Backend') {
            agent any
            steps {
                withAWS(region:'ap-southeast-1',credentials:'aws-credential') {
                    sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${REPO_URI}"
                    sh "docker build -t ${BACKEND_REPO} backend/"
                    sh "docker tag ${BACKEND_REPO}:latest ${REPO_URI}/${BACKEND_REPO}:${IMAGE_TAG}"
                    sh "docker push ${REPO_URI}/${BACKEND_REPO}:${IMAGE_TAG}"
                }
            }
        }
    }
}
