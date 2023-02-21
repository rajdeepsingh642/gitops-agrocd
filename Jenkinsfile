pipeline{
agent any
stages{
    stage('Gitcode'){
        steps{
            git branch: 'main', url: 'https://github.com/rajdeepsingh642/gitops-agrocd.git'
         }
    }
    stage('Build image') {
        steps{
            sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
            sh 'docker image tag $JOB_NAME:v1.$BUILD_ID rajdeepsingh642/$JOB_NAME:v1.$BUILD_ID'
            sh 'docker image tag $JOB_NAME:v1.$BUILD_ID rajdeepsingh642/$JOB_NAME:latest'
           }
        }
    stage('Push image') {
        steps{
        withCredentials([string(credentialsId: 'rajdeepsingh642', variable: 'dockerhub')]) {
        sh "docker login -u rajdeepsingh642 -p ${dockerhub}"
        sh 'docker image push rajdeepsingh642/$JOB_NAME:latest'
        }
        }
    }
     
    stage('Trigger') {
        steps{
               sh 'echo "triggering updatemanifestjob"'
                 build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_ID)]
               }
    }
}
}
