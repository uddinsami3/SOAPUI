pipeline{
	agent any
	stages {
		stage('GIT Checkout') {
			steps {
				node('master'){
					git '${GIT_REPO}'
					//bat '%SOAPUI_HOME%\\testrunner.bat -sTestSuite1 -raj -fC:\\Users\\syednaseru\\Desktop\\SoapUIResults\\reports StudentSampleAPI-soapui-project.xml'
				}
			}
		}
		stage('Execute TestCases') {
			steps {
				node('master'){
					bat '%SOAPUI_HOME%\\testrunner.bat -s%TEST_SUITE% -raj -f%REPORTS_OUTPUT% %PROJECT%'
				}
			}
		}
		stage('Compress Reports') {
			steps {
				node('master'){
					bat "del reports.zip"
					zip zipFile: 'reports.zip', archive: false, dir: 'C:\\Users\\syednaseru\\Desktop\\SoapUIResults\\reports'
				}
			}
		}
	}
	post {
        failure {
            emailext attachmentsPattern: 'reports.zip', body: "Please visit ${env.BUILD_URL} for further information.", 
                    subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Failed", 
                    to: "naser.maveric@gmail.com"
            }
         success {
               emailext attachmentsPattern: 'reports.zip', body: "Please visit ${env.BUILD_URL} for further information.", 
                    subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful", 
                   to: "naser.maveric@gmail.com"
          }      
    }
}