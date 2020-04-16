node {
	stage("Build") {
		sh 'echo "-----------------------------------------BUILD-----------------------------------------"'
		checkout scm
		try{
			sh 'docker rmi oriexsol/my_app:latest'
		}catch(all){
			sh 'echo "image: oriexsol/my_app:latest out"'
		}
		sh 'docker build -t oriexsol/my_app:latest .'
	}
	stage("Test") {
		sh 'echo "-----------------------------------------Test------------------------------------------"'
		sh 'docker run --name dev_my_app -p 80:80 -dit oriexsol/my_app:latest'
		sh 'chmod +x isalive.sh'
		def isalive = sh (script: "./isalive.sh", returnStdout: true)
		sh 'docker rm -f dev_my_app'
		if ("${isalive}") {
			echo "Testing completed successfully!"
		}
		else {
			echo "Failed The Testing"
			currentBuild.result = 'FAILURE'
		}
	}
	stage ("Deploy") {
		try{
			sh 'docker rm -f my_app_prod'
		}catch(all){
			sh 'echo "image: oriexsol/my_app:latest out"'
		}
		sh 'docker build -t oriexsol/my_app_prod:latest .'
		sh 'docker rm -f my_app_prod'
		sh 'docker run --name my_app_prod -p 80:80 -dit oriexsol/my_app_prod:latest'
	}
}
