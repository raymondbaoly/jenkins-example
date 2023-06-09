pipeline {
    agent any

    environment {
        // Please update your own registry here
        REGISTRY = 'registry.baohoi.com'
        REGISTRY_IMAGE = "$REGISTRY/private/jenkins-example"
        DOCKERFILE_PATH = 'Dockerfile'

        REGISTRY_USER = credentials('registryUser')
        REGISTRY_PASSWORD = credentials('registryPassword')

        CURRENT_BUILD_NUMBER = "${currentBuild.number}"
        GIT_COMMIT_SHORT = sh(returnStdout: true, script: "git rev-parse --short ${GIT_COMMIT}").trim()
    }

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t $REGISTRY_IMAGE:$GIT_COMMIT_SHORT-jenkins-$CURRENT_BUILD_NUMBER -f $DOCKERFILE_PATH .'
            }
        }
        stage('Push') {
            steps {
                sh 'docker login -u $REGISTRY_USER -p $REGISTRY_PASSWORD $REGISTRY'
                sh 'docker push $REGISTRY_IMAGE:$GIT_COMMIT_SHORT-jenkins-$CURRENT_BUILD_NUMBER'
            }
        }

    }

}
