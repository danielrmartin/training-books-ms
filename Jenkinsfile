def serviceName = "training-books-ms"
def registry = "docker-registry:5000"

node("docker") {
    git "https://github.com/cloudbees/${serviceName}.git"
   // flow = load "/mnt/scripts/pipeline-common.groovy"
    common.runPreDeploymentTests(serviceName, registry)
    common.build(serviceName, registry)
}
checkpoint "deploy"
node("docker") {
    common.deploy(serviceName, registry)
    common.runPostDeploymentTests(serviceName, registry, "http://[IP]:8081")
}
