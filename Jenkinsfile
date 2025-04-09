pipeline {
    agent any

    stages {
        stage("Build") {
            steps {
                sh "npm ci"
                sh "npm build"                    
            }
        }

        stage("Test") {
            steps {
                wrap([$class: "Xvfb", debug: true, autoDisplayName: true]) {
                    sh "npm test"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "mkdir -p ~/.ssh"
                sh "ssh-keyscan 157.245.150.85 >> ~/.ssh/known_hosts"
                sh "rsync -avz ./dist ci@157.245.150.85:/var/www/html --progress"
            }
        }
    }
}
