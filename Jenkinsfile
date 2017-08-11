pipeline{
    agent none
stages{
stage ('checkout')
{
  agent {label 'docker'}
  steps{
      git "http://student-0.lab-pipeline.class-dryrun.cloudbees-training.com:5000/gitserver/butler/training-books-ms"
  }
}
stage('pre-deployment test')
        {agent {label 'docker'}
        environment {
        dir=pwd()
           }
            steps{
                sh "docker run --rm -v $dir/target/scala-2.10:/source/target/scala-2.10 -v db:/data/db docker-registry:5000/training-books-ms-tests ./run_tests.sh"
            }
        }
        stage ('build')
        {agent {label 'docker'}
            steps
            {
                    sh "docker build -t docker-registry:5000/books-ms ."
                    sh "docker push docker-registry:5000/books-ms"
                    stash includes: "docker-compose*.yml", name: "docker-compose"
            }
        }
        
  stage('Pull Production images'){
       agent {label 'production' }
       steps{
        parallel (service:{
            sh "docker pull docker-registry:5000/training-books-ms"
            },
            db:{
        sh "docker pull mongo"
            })
       }
  }
        stage("deploy") {
        agent {label 'production'}
       steps{
        unstash "docker-compose"
        //sh "docker pull docker-registry:5000/training-books-ms"
        //sh "docker pull mongo"
        sh "docker-compose -p books-ms up -d app"
        sleep 2
    }
}
stage("post-deployment tests"){
        agent { label 'docker'}
       steps{
          // 172.17.0.1 is the IP of the host reachable from all containers
          withEnv(["TEST_TYPE=integ", "DOMAIN=http://172.17.0.1:8081"]) {
         sh "docker run --rm docker-registry:5000/training-books-ms-tests ./run_tests.sh"
          }
        }
      }
}
}
