pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        GIT_REPO = 'https://github.com/sahaavr/saha.git'
        DEPLOY_PATH = '/var/www/html/saha'
        MYSQL_HOST = 'database-1.c3ca0kky63jx.ap-south-1.rds.amazonaws.com'
        MYSQL_PORT = '3306'
        MYSQL_USER = 'admin'
        MYSQL_PASSWORD = 'admin123'
        MYSQL_DB = 'database-1'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: "${env.GIT_REPO}"
            }
        }

        stage('Build') {
            steps {
                echo 'No build required for static HTML...'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying index.html to ${env.DEPLOY_PATH}"
                sh "sudo cp -v index.html ${DEPLOY_PATH}/"
            }
        }

        stage('Test DB Connection') {
            steps {
                echo "Testing MariaDB connection..."
                sh '''
                export DEBIAN_FRONTEND=noninteractive

                if ! command -v mysql >/dev/null; then
                    echo $JENKINS_SUDO_PASSWORD | sudo -S apt-get update
                    echo $JENKINS_SUDO_PASSWORD | sudo -S apt-get install -y mariadb-client
                fi

                mysql -h $MYSQL_HOST -P $MYSQL_PORT -u$MYSQL_USER -p$MYSQL_PASSWORD -e 'SHOW DATABASES;'
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment and DB test successful!'
        }
        failure {
            echo '❌ Something went wrong in the pipeline!'
        }
    }
}
