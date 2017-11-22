def BRANCH = env.BRANCH_NAME
def clone() {
	   sh """
		cd /home/jnorrie
                if [ -d "${BRANCH}" ]; then
                rm -rf ${BRANCH}
                echo "build already exists, cleaning..."
                fi
		mkdir ${BRANCH}
		cd ${BRANCH}
	   	git clone -b ${BRANCH} https://github.com/JenkTest/Jenk
		echo "worked"
		"""
	}

pipeline {
agent{
	label 'NumeroUno'
	}
  stages {
      stage('Clone Branch'){
         steps {
            	echo "We are currently working on branch: ${BRANCH}"
		clone()
         }
    }
    stage('Build Cmake'){
        steps{
            
            sh  """
                cd /home/jnorrie/${BRANCH}
		cmake Jenk
                echo "Build complete, cleaning project"
		cd ..
		rm -rf ${BRANCH}
		echo "Build Removed"
                """
        }
    }
      stage('Test') {
        steps {
          script {
              // change to 'UNSTABLE' OR 'FAILED' to test the behaviour 
              currentBuild.result = 'SUCCESS'
          }
        }
      }
  }
  post {
        always {
          step([$class: 'Mailer',
            notifyEveryUnstableBuild: true,
            recipients: 'jenkenstest@gmail.com',
            sendToIndividuals: true])
        }
  }
}
