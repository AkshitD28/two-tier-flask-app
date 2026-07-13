@Library("Shared") _
pipeline{
    
    agent { label "dev" };
    
    stages{
        stage("Code Clone"){
            steps{
              script{
                  clone("https://github.com/AkshitD28/two-tier-flask-app.git", "main")
              }
            }
        }
        stage("Trivy file system scan"){
            steps{
               script{
                   trivy_fs()
               }
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
            
        }
        stage("Test"){
            steps{
                echo "Developer / Tester tests likh ke dega..."
            }
            
        }
        stage("Push to Docker Hub") {
            steps {
                script{
                    docker_push("dockerHubCreds", "two-tier-flask-app")
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
    post{
        success{
            script{
                emailext (
            from: 'akshitdubey2808@gmail.com',
            to: 'akshitdubey2808@gmail.com',
            subject: 'Build Success For Demo CiCd app',
            body: 'Build is success for cicd app',
            mimeType: 'text/plain' // Explicitly forces raw text to bypass log attachment parsers
        )
            }
        }
        failure{
            script{
                emailext from: 'akshitdubey2808@gmail.com',
                    to: 'akshitdubey2808@gmail.com',
                    body: 'Build is Failed for cicd app',
                    subject: 'Build Failed for Demo CiCd app'
            }
        }
    }
}
