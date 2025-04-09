pipeline {
    agent any

    stages {
        stage("Build") {
            steps {
                sh "distrobox enter pw -- npm ci"
                sh "distrobox enter pw -- npm run build"                    
            }
        }

        stage("Test") {
            steps {
                wrap([$class: "Xvfb", debug: true, autoDisplayName: true]) {
                    sh "distrobox enter pw -- npm test"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "mkdir -p ~/.ssh"
                sh "ssh-keyscan 157.245.150.85 >> ~/.ssh/known_hosts"
                sh "rsync -avzd ./build/ root@157.245.150.85:/srv/html/ --progress"
            }
        }
    }
}
