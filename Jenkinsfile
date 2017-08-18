def serviceName = "training-books-ms"
def registry = "localhost:5000"
//def flow

node("docker") {
    git "https://github.com/cloudbees/${serviceName}.git"
//    flow = load "/mnt/scripts/pipeline-common.groovy"
    common.runPreDeploymentTests(serviceName, registry)
    common.build(serviceName, registry)
}
checkpoint "deploy"
node("docker") {
    common.deploy(serviceName, registry)
    common.runPostDeploymentTests(serviceName, registry, "http://[IP]:8081")
}
