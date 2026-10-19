
pipeline {
    agent {
        label 'slave1'
    }

    environment {
        DB_ROOT_PASSWORD = credentials('mysql-root-password')
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Deploy App and Database') {
            steps {
                sh 'docker-compose up -d'
            }
        }

        stage('Use Database and Show Tables') {
            steps {
                sh '''
                    docker compose exec -t db sh -c \
                    'MYSQL_PWD="$MYSQL_ROOT_PASSWORD" mysql -uroot cricket_db -e "SELECT DATABASE(); SHOW TABLES;"'
                '''
            }
        }
    }

    post {
        success {
            echo 'Application deployed and database verified successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the Jenkins Console Output.'
        }
    }
}
