def clone(BRANCH) {
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

def build(BRANCH) {
		sh  """
        cd /home/jnorrie/${BRANCH}
	cmake Jenk
        echo "Build complete, cleaning project"
	cd ..
	rm -rf ${BRANCH}
	echo "Build Removed"
        """
	}

pipeline {
agent{
	label 'NumeroUno'
	}
	
  stages {
      stage('Clone Branch'){
	      steps {
		     echo "We are currently working on branch: ${env.BRANCH_NAME}" 
		     clone(env.BRANCH_NAME)
         	}
  		post {
	  		success {echo "Build complete"}
			failure {echo "Failure whilst cloning branch"}
  		}	  
  	}  
	  
    stage('Build Cmake'){
    	steps{
		echo "Building branch"
		build(env.BRANCH_NAME)
		echo "Branch built."
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

