pipeline {
  agent any

  environment {
    S3_BUCKET = "bhavya-bhavya-apple"
    AWS_REGION = "us-east-1"
  }

  stages {
    stage('Checkout') {
      steps {
        git  branch:'main'
              url: 'https://github.com/bhavyabandlam/frontend.git'
          
      }
    }

    stage('Build') {
      steps {
        sh '''
          cd frontend
          npm install
          npm run build
        '''
      }
    }

    stage('Upload to S3') {
      steps {
        sh '''
          aws s3 sync frontend/build s3://$S3_BUCKET --delete
        '''
      }
    }

    stage('Invalidate CloudFront') {
      steps {
        sh '''
          aws cloudfront create-invalidation \
          --distribution-id E2LSDHNRWD3O43 \
          --paths "/*"
        '''
      }
    }
  }
}
