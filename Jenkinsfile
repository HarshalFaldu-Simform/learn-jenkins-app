pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    // No args, use default node user (uid 1000)
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Current user:"
                    whoami
                    id

                    echo "Fixing any ownership issues (if needed)..."
                    if [ "$(id -u)" -eq 1000 ]; then
                      chown -R node:node .
                    fi

                    echo "Cleaning and installing dependencies..."
                    rm -rf node_modules
                    npm ci

                    echo "Building project..."
                    npm run build

                    echo "Listing files after build:"
                    ls -la
                '''
            }
        }
    }
}
