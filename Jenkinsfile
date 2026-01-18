pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'rengafrontends3'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/renga86dvm/frontend-repo.git',
                    credentialsId: 'aws-creds'
            }
        }

        stage('Build React') {
            steps {
                sh 'chmod +x build.sh'
                sh './build.sh'
            }
        }

        stage('Deploy to S3 & Invalidate CloudFront') {
            steps {
                withAWS(credentials: 'aws-creds', region: 'us-east-1') {
                    sh 'aws s3 sync build/ s3://$S3_BUCKET --delete'
                    sh 'aws cloudfront create-invalidation --distribution-id E28B08W45JIKSL --paths "/*"'
                }
            }
        }
    }
}
