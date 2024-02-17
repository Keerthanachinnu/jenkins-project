node{
    withDockerContainer(args: '--user root -v /var/run/docker.sock:/var/run/docker.sock', image: 'python:3.9'){
        stage('Checkout') {
            git branch: 'main', url:'https://github.com/Keerthanachinnu/jenkins-project.git'
            sh 'ls'
        }
        stage('Build and Test') {
            sh 'python -m pip install --upgrade pip'
            sh 'pip install -r requirements.txt'
            sh 'python manage.py test'
        }
        stage('Static Code Analysis') {
            // try {
            //     sh 'flake8'
            // }
            // catch (err) {
            //     def errorMessage = "${err}"
            //     error "Static code analysis failed: ${errorMessage}"
            // }
            // finally {
            //     currentBuild.result = 'SUCCESS'
            // }
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh 'flake8'
            }
        }
        stage('Build and Push Docker Image'){
            withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                sh '''
                    docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                    docker build -t $DOCKER_USERNAME/todopythonapp:${BUILD_NUMBER} .
                    docker push $DOCKER_USERNAME/todopythonapp:${BUILD_NUMBER}
                '''
            }
        }
          stage('Update Deployment File'){
            withCredentials([usernamePassword(credentialsId: 'github-creds', passwordVariable: 'GITHUB_TOKEN', usernameVariable: 'GITHUB_USERNAME')]) {
                sh '''
                    git config user.email "keerthanakeerthu31.kk@gmail.com"
                    git config user.name "keerthana"
                    BUILD_NUMBER=${BUILD_NUMBER}
                    sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" manifests/deployment.yml
                    git add manifests/deployment.yml
                    git commit -m "Update deployment image to version ${BUILD_NUMBER}"
                    git push https://${GITHUB_TOKEN}@github.com/${GITHUB_USERNAME}/jenkins-project HEAD:main
                '''
            }
        }
    }
}