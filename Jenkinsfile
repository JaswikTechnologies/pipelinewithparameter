pipeline {
    agent any
    parameters {
        booleanParam(name: 'VERSION',defaultValue: 'true', description: 'version to deploy on prod')
        choice(name: 'VERSION',choices: ['1.1.0', '1.2.0','1.3.0'], description: '')
    }
    stages {
      stage("git clone")
        {
            steps{

                sh 'rm -rf pipelinewithparameter*'
                sh 'git clone https://github.com/JaswikTechnologies/pipelinewithparameter.git'

            }
            
        }

        stage('Build Docker Image') {
          steps {
            sh 'docker build -t jaswiktechnologiesdocker/nginx:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push jaswiktechnologiesdocker/nginx:${BUILD_NUMBER}'
           
          }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.1.1.21:2375 stop masterwebapp1 || true'
            sh    'docker -H tcp://10.1.1.21:2375 run --rm -dit --name masterwebapp1 --hostname masterwebapp1 -p 8001:80 jaswiktechnologiesdocker/nginx:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.1.1.21:8001'
         
          }
        }

    }
}
