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
	  		success {echo "Branch cloned."}
			failure {echo "Failure whilst cloning branch."}
  		}	  
  	}  
	  
    stage('Build Cmake'){
    	steps{
		echo "Building branch."
		build(env.BRANCH_NAME)
	}
	    post{
		    success{echo "Branch built."}
		    failure {echo "Error in build."}
	    }
  	}
  
  }
  post {
        always {
	   	publishHTML target: [
              		allowMissing: false,
              		alwaysLinkToLastBuild: false,
              		keepAll: true,
              		reportDir: 'coverage',
              		reportFiles: 'index.html',
              		reportName: 'Report'
            		]
        }
  }
}

