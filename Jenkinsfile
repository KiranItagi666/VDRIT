pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/youruser/vdrt-devops-demo.git', branch: 'main'
      }
    }

    stage('Build/Validate') {
      steps {
        // simple validation: ensure index.html exists
        sh 'test -f index.html && echo "index.html found" || (echo "index.html missing" && exit 1)'
      }
    }

    stage('Deploy to Webroot') {
      steps {
        // copy index.html to nginx webroot
        // use sudo if required; here we assume jenkins user can write to /var/www/html
        sh 'cp -f index.html /var/www/html/index.html'
      }
    }
  }

  post {
    success {
      echo "Deployment successful. Visit http://$env.BUILD_URL/"
    }
    failure {
      echo "Pipeline failed"
    }
  }
}


