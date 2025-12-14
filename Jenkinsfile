pipeline {
    agent {
      label 'puppet'
    }

    post {
        always {
            deleteDir() /* clean up our workspace */
        }
        success {
            updateGitlabCommitStatus state: 'success'
        }
        failure {
            updateGitlabCommitStatus state: 'failed'
            step([$class: 'Mailer', notifyEveryUnstableBuild: true, recipients: 'support@confdroid.com', sendToIndividuals: true])
        }
    }

    options {
          gitLabConnection('gitlab.confdroid.com')
    }

    stages {

      stage('pull master') {
        steps {
          sshagent(['edd05eb6-26b5-4c7b-a5cc-ea2ab899f4fa']) {
            sh '''
                git config user.name "Jenkins Server"
                git config user.email jenkins@confdroid.com
                # Ensure we're on the development branch (triggered by push)
                git checkout development
                # Create jenkins branch from development
                git checkout -b jenkins-build-$BUILD_NUMBER
                # Optionally merge master into jenkins to ensure compatibility
                git merge origin/master --no-ff || { echo "Merge conflict detected"; exit 1; }
            '''
        }
      }
    }

      stage('SonarScan') {
         steps {
            withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
              sh '''
              /opt/sonar-scanner/bin/sonar-scanner \
                -Dsonar.projectKey=puppet_collection \
                -Dsonar.sources=. \
                -Dsonar.host.url=https://sonarqube.confdroid.com \
                -Dsonar.token=$SONAR_TOKEN
                '''
              }
            }
          }

      stage('update repo') {
        steps {
          sshagent(['edd05eb6-26b5-4c7b-a5cc-ea2ab899f4fa']) {
            sh '''
                 git config user.name "Jenkins Server"
                git config user.email jenkins@confdroid.com
                git add -A && git commit -am "Recommit for updates in build $BUILD_NUMBER" || echo "No changes to commit"
                git push origin HEAD:master
            '''
        }
      }
    }

         stage('Mirror to Gitea') {
             steps {
                withCredentials([usernamePassword(
                credentialsId: 'Jenkins-gitea',
                usernameVariable: 'GITEA_USER',
                passwordVariable: 'GITEA_TOKEN')]) {
                script {
                    // Checkout from GitLab (already done implicitly)
                    sh '''
                        git checkout master
                        git pull origin master
                        git branch -D development
                        git branch -D jenkins-build-$BUILD_NUMBER
                        git rm -f Jenkinsfile
                        git commit --amend --no-edit --allow-empty
                        git remote add master https://gitea.confdroid.com/confdroid/puppet_collection.git
                        git -c credential.helper="!f() { echo username=${GITEA_USER}; echo password=${GITEA_TOKEN}; }; f" \
                        push master --mirror
                    '''
					}
				}
			}
		}

         stage('Mirror to Github') {
             steps {
              sshagent(['edd05eb6-26b5-4c7b-a5cc-ea2ab899f4fa']) {
                sh '''
                  git remote set-url --push master git@github.com:grizzlycoda/puppet_collection.git
                  push master --mirror
                '''
        }
      }
	  }
  }
}
