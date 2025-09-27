// Jenkinsfile for an Angular MFE deploying to a live S3 URL

pipeline {
    agent any

    environment {
        S3_BUCKET_NAME     = 'my-mfe-demo-2025'
        AWS_CREDENTIALS_ID = 'aws-s3-credentials'
        AWS_REGION         = 'ap-south-1'
        ANGULAR_PROJECT_NAME = 'host_app'
    }

    stages {
        stage('Build Angular App') {
            steps {
                echo 'Installing dependencies and building the application...'
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Deploy to Live S3 URL') {
            when {
                branch 'master'
            }
            steps {
                echo "Deploying to live URL for bucket: ${env.S3_BUCKET_NAME}"
                withAWS(credentials: env.AWS_CREDENTIALS_ID, region: env.AWS_REGION) {
                    s3Upload(
                        file: "dist/${env.ANGULAR_PROJECT_NAME}/**",
                        bucket: env.S3_BUCKET_NAME,
                        path: '/'
                    )
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
            echo "✅ View your live MFE at: http://${env.S3_BUCKET_NAME}.s3-website.${env.AWS_REGION}.amazonaws.com"
        }
    }
}