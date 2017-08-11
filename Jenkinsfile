pipeline{
    agent {label 'cd'}
    environment{
def serviceName = "training-books-ms"
def registry = "docker-registry:5000"
}
stages{
    stage('checkout'){
        steps{
            git branch: 'pipeline',url: "https://github.com/cloudbees/${serviceName}.git"
        }
    }
    stage('pre-deployment')
    {
        steps{script{
    common.runPreDeploymentTests(serviceName, registry)
        }}
    }
    stage('build'){
        steps{script{
            common.build(serviceName, registry)
        }}
    }
    stage('deploy') {
        steps{script{
    common.deploy(serviceName, registry)
        }}
    }
    stage('post-deployment test'){
        steps{script{
            common.runPostDeploymentTests(serviceName, registry, "http://172.17.0.1:8081")
        }}
    }
}
}
