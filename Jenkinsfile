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
/*node("docker") {
    git "https://github.com/cloudbees/${serviceName}.git"
    common.runPreDeploymentTests(serviceName, registry)
    common.build(serviceName, registry)*/

}
