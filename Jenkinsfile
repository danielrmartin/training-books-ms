pipeline{
    agent none
stages{
    
    stage('pre-deployment')
    {
	agent {label 'docker'}
    	environment{
	def serviceName = "training-books-ms"
	def registry = "docker-registry:5000"
	}
        steps{script{ common.runPreDeploymentTests(serviceName, registry)}}
    }
    stage('build'){
     agent {label 'docker'}
        steps{script{common.build(serviceName, registry)}}
    }
    stage('deploy') {
	agent {label 'production'}
    	environment{
	def serviceName = "training-books-ms"
	def registry = "docker-registry:5000"
}
        steps{script{common.deploy(serviceName, registry)}}
    }
    stage('post-deployment test'){
      agent {label 'docker'}
        steps{script{common.runPostDeploymentTests(serviceName, registry) }}
    }
}
}
