pipeline {
  agent any

  environment {
    IMAGE_NAME = "imohan21/flaskapp:v1"
  }
  stages {
    stage('GIT CHECKOUT') {
      steps {
          git branch: 'main'
              url: 'https://github.com/mohan-m21/flask.git'
      }
    }
      stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'echo "Running tests..."'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS')]) {
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh 'docker push $IMAGE_NAME'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 5000:5000 $IMAGE_NAME'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Successful'
        }
        failure {
            echo 'Build Failed'
        }
  }
}
