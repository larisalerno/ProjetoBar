pipeline {
    agent any 
    stages {
        stage('Build') {
            steps {
                echo 'Passo de build!'
                // Run the maven build
                script {
                    // Get the Maven tool.
                    // ** NOTE: This 'M3' Maven tool must be configured
                    // **       in the global configuration.
                    echo 'Pulling...' + env.BRANCH_NAME
                    def mvnHome = tool 'Maven 3.3.9'
                    if (isUnix()) {
                        def targetVersion = getDevVersion()
                        print 'target build version...'
                        print targetVersion
                        sh "'${mvnHome}/bin/mvn' -Dintegration-tests.skip=true -Dbuild.number=${targetVersion} clean package"
                        def pom = readMavenPom file: 'pom.xml'
                        // get the current development version 
                        developmentArtifactVersion = "${pom.version}-${targetVersion}"
                        print pom.version
                        // execute the unit testing and collect the reports
                        junit '**//*target/surefire-reports/TEST-*.xml'
                        archive 'target*//*.jar'
                    } else {
                        bat(/"${mvnHome}\bin\mvn" -Dintegration-tests.skip=true clean package/)
                        def pom = readMavenPom file: 'pom.xml'
                        print pom.version
                        junit '**//*target/surefire-reports/TEST-*.xml'
                        archive 'target*//*.jar'
                    }

		}
		}
	}
        stage('Teste') {
            steps {
                echo 'Passo de teste automatizado!' 
            }
        }
        stage('Deploy') {
            steps {
                echo 'Passo de deploy!' 
            }
        }
		
    }
}
