pipeline {
    agent any
	
    parameters {
        booleanParam(name: "RUN_INTEGRATION_TESTS", defaultValue: true)
    }
    stages {
        stage('Tests') {
            parallel {
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
				stage('Deploy') {
					when {
						expression { env.GIT_BRANCH == 'origin/main' }
						beforeInput true
					}
					input {
						message 'Deploy the application?'
					}
					steps {
						echo 'Deploying...'
					}
				}
            }
        }
    }
}