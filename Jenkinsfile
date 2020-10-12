pipeline {
	agent any
	stages {
		stage ("Git checkout"){
			steps {
				git branch: "master",
					url: "https://github.com/PrabhuVignesh/movie-crud-flask.git"
				sh "ls"
			}
		}
		stage ("Python Flask Prepare"){
			steps {
				sh "pip3 install -r requirements.txt"
			}

		}
		stage ("Unit Test"){
			steps{
				sh "python3 test_basic.py"
			}
		}
		stage ("Python Bandit Security Scan"){
			steps{
				sh "docker run --rm -v ${env.WORKSPACE}:/code opensorcery/bandit --exit-zero -f json -o bandit_report.json -r /code"
				sh "ls"
			}
		}
		stage ("Dependency Check with Python Safety"){
			steps{
				sh "docker run --rm --volume \$(pwd) pyupio/safety:latest safety check"
				sh "docker run --rm --volume \$(pwd) pyupio/safety:latest safety check --json > report.json"
			}
		}
		stage ("Static Analysis with python-taint"){
			steps{
				sh "docker run --rm --volume \$(pwd) vickyrajagopal/python-taint-docker pyt ."
			}
		}
		stage ("sonar-publish"){
			steps {
				echo "===========Performing Sonar Scan============"
				sh "${tool("sonarqube")}/bin/sonar-scanner"
			}
		}
		
	}
}
