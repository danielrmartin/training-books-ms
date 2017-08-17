def serviceName = "training-books-ms"
def registry = "localhost:5000"


node("docker") {
    git "https://github.com/cloudbees/${serviceName}.git"
    common.runPreDeploymentTests(serviceName, registry)
    common.build(serviceName, registry)
}
