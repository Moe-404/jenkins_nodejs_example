pipeline {
    agent any

    stages {
        stage('Fetch Repo') {
            steps {
                git url: 'https://github.com/Moe-404/jenkins_nodejs_example', branch: 'master',
                credentialsId: 'github-token'
            }
        }//end of stage
        stage('CI') {
            steps {
                withCredentials([usernameColonPassword(credentialsId: 'dockerHub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                    docker login -u ${USERNAME} -p ${PASSWORD}
                    docker build . -f dockerfile -t moe404/nodejs-app:${BUILD_NUMBER}
                    docker push moe404/nodejs-app:${BUILD_NUMBER}
                """
                }
            }
        }//end of stage
        stage('CD') {
            steps {
                withCredentials([usernameColonPassword(credentialsId: 'dockerHub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                    docker login -u ${USERNAME} -p ${PASSWORD}
                    docker run -d -p 3000:3000 moe404/nodejs-app:${BUILD_NUMBER}
                """
                }
            }
        }//end of stage
    }
}
