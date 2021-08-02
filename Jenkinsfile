pipeline {
  agent any
  environment {
    COURSE = 'Calgary DevOps'
    BRANCH = 'main'
    WWWROOT = '/var/www/html'
    SSHUSER = 'alexandr'
    WORKSPACE = '/home/alexandr/devops'
  }
  stages {
    stage('Audit Tools') {
      steps {
        echo "Audit all tools to be use on this pipeline ${BRANCH}"
        sh "git --version"
        //sh "node --version"
        //sh "npm --version"
        //sh "ng --version"
        sh "ansible --version"
        sh "whoami"
        echo "Workspace Folder: ${WORKSPACE}"
      }
    }
    stage('Install packages') {
      steps {
        sh "git pull origin ${BRANCH}"
      }
    }
    stage('Install Front-End Packages') {
      steps {
        dir("${WORKSPACE}/conduit-ui") {
          echo "Install conduit UI packages"
          //sh "npm install"
        }
      }
    }
    stage('Run linting') {
      steps {
        dir("${WORKSPACE}/conduit-ui") {
          echo "npm run lint"
        }
      }
    }
    stage('Build UI') {
      steps {
        dir("${WORKSPACE}/conduit-ui") {
          sh "npm run build"
        }
      }
    }
    stage('Deploy app to WEB1') {
      steps {
        sh "ssh web1 rm -rf /home/${SSHUSER}/conduit"
        sh "scp -r ${WORKSPACE}/conduit-ui/dist web1:/home/${SSHUSER}/conduit"
        sh "ssh web1 sudo rm -rf ${WWWROOT}/conduit"
        sh "ssh web1 sudo cp -r /home/${SSHUSER}/conduit ${WWWROOT}/conduit"
      }
    }
  }
}