pipeline {
    agent any
	
    parameters {
        booleanParam(name: "RUN_INTEGRATION_TESTS", defaultValue: true)
    }
    stages {
			stage('Unit tests') {
				steps {
					sh './mvnw test -D testGroups=unit'
				}
			}
			stage('Integration tests') {
				when {
					expression { return params.RUN_INTEGRATION_TESTS }
				}

				steps {
					sh './mvnw test -D testGroups=integration'
				}
			}
			stage('Build') {
				steps {
					script {
						try {
							sh './mvnw package -D skipTests'
						} catch (ex) {
							echo "Error while generating JAR file"
							throw ex
						}
					}
				}
			}
			stage('Deploy') {
				when {
					expression { env.GIT_BRANCH == 'origin/main' }
					beforeInput true
				}
				steps {
					echo 'Deploying...'
				}
			}
    }
}