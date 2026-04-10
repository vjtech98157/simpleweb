pipeline {
    agent any

    environment {
        PROJECT_NAME = "simpleweb_v2"   // 👈 your new folder
        BASE_DIR     = "/opt/projects"
        PROJECT_DIR  = "${BASE_DIR}/${PROJECT_NAME}"
        NGINX_DIR    = "/var/www/html"
        REPO_URL     = "https://github.com/vjtech98157/simpleweb.git"
    }

    stages {

        stage('Setup Permissions') {
            steps {
                echo "🔐 Setting up directories..."
                sh '''
                    sudo mkdir -p $BASE_DIR
                    sudo chown -R jenkins:jenkins $BASE_DIR
                '''
            }
        }

        stage('Clean Old Code') {
            steps {
                echo "🧹 Cleaning project directory..."
                sh '''
                    rm -rf $PROJECT_DIR
                '''
            }
        }

        stage('Clone Fresh Code') {
            steps {
                echo "📥 Cloning latest code..."
                sh '''
                    git clone $REPO_URL $PROJECT_DIR
                '''
            }
        }

        stage('Validate File') {
            steps {
                echo "🔍 Checking index.html..."
                sh '''
                    if [ ! -f "$PROJECT_DIR/index.html" ]; then
                        echo "❌ index.html not found!"
                        exit 1
                    fi
                '''
            }
        }

        stage('Deploy to Nginx') {
            steps {
                echo "🚀 Deploying to Nginx..."
                sh '''
                    sudo cp $PROJECT_DIR/index.html $NGINX_DIR/
                '''
            }
        }

        stage('Reload Nginx') {
            steps {
                echo "🔄 Reloading Nginx..."
                sh '''
                    sudo systemctl reload nginx
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "✅ Verifying deployment..."
                sh '''
                    curl -I http://localhost
                '''
            }
        }
    }

    post {
        success {
            echo "🎉 Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
