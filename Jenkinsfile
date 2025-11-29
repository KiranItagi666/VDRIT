pipeline {
  agent any

  environment {
    WEBROOT = "/var/www/html"
    TARGET = "${env.WEBROOT}/index.html"
  }

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/KiranItagi666/VDRIT.git', credentialsId: 'github-pat', branch: 'main'
      }
    }

    stage('Build/Validate') {
      steps {
        sh 'test -f index.html && echo "index.html found" || (echo "index.html missing" && exit 1)'
      }
    }

    stage('Deploy to Webroot') {
      steps {
        sh '''
          echo "Deploying index.html to ${TARGET}"
          sudo /bin/cp -f index.html "${TARGET}"
          sudo /bin/chown www-data:www-data "${TARGET}"
          sudo /bin/chmod 644 "${TARGET}"
          echo "Deployed: $(ls -l ${TARGET})"
        '''
      }
    }

    stage('Verify') {
      steps {
        sh 'curl -I http://localhost/ || true'
      }
    }
  }

  post {
    success { echo "Deployment successful" }
    failure { echo "Pipeline failed" }
  }
}
