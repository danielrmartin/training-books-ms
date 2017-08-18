pipeline{
    agent {label 'docker'}
    environment{
def serviceName = "training-books-ms"
def registry = "docker-registry:5000"
}
    stages{
        stage("Pre-Deployment Tests")
        {
            steps{script{common.runPreDeploymentTests(serviceName, registry)}}
        }
        stage('build'){
            steps{script{common.build(serviceName, registry)}}
        }
    }
    stage( "Deploy")
    {
        steps{script{common.deploy(serviceName, registry)}}
    }
    stage ('Post Deployment Test')
    {
        steps{script{common.runPostDeploymentTests(serviceName, registry)}}
    }
}
