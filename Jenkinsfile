pipeline{
    agent {label 'docker'}
    stages{
    stage('checkout'){
        steps{
    git branch: 'pipeline', url: 'https://github.com/cloudbees/training-books-ms'
        }}
        
     stage('test') {
        agent{ docker {
      image 'golang'
      label 'docker'
      args  '-u 0:0' }}
  steps{
      sh 'ln -s $PWD /go/src/docker-flow'
      sh 'cd /go/src/docker-flow && go get -t && go test --cover -v'
      sh 'cd /go/src/docker-flow && go build -v -o docker-flow-proxy'
     }
     }
    stage ('build'){
       agent {label 'docker'}
       steps{
           sh "docker build -t docker-registry:5000/docker-flow-proxy ."
           sh "docker push docker-registry:5000/docker-flow-proxy"
           archive 'docker-flow-proxy'
       }
   }

   stage ('deployment checkpoint'){
       agent none
       steps{ checkpoint ('deploy')
       }
   }

  stage ('deploy')
   {
       agent {label 'production'}
       steps{
           script{
             try{
           sh "docker rm -f docker-flow-proxy"
             }
             catch (e){}
             }
           sh "docker run -d --name docker-flow-proxy -p 9081:80 -p 9082:8080 docker-registry:5000/docker-flow-proxy"
       }
   }
}
}
