pipeline {
    agent any
     environment {
        PATH = "/opt/homebrew/bin:${env.PATH}"
    }
    stages {
        stage('Build') {
            steps {
                 echo "Build.........."
                sh 'mvn -f hello-app/pom.xml -B -DskipTests clean package'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts....."
                    archiveArtifacts artifacts: '**/*.jar'
                }
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -f hello-app/pom.xml test'
            }
            post {
                always {
                    junit 'hello-app/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Deploy to Local Tomcat') {
            steps {
                 echo "Ansible...."
                sh 'ansible-playbook -i ~/ansible-deployment/hosts ~/ansible-deployment/deploy.yml'
            }
        }

       
    }
}
