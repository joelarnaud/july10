pipeline {
    agent any
    triggers {
        cron('H */1 * * *')
    }
             
    tools {
        maven 'M2_HOME' 
    }
    stages {
      stage('Build'){
        steps {
          echo "Build step"
          sh 'mvn clean'
          sh 'mvn install'
          sh 'mvn package'
        }
      }
        stage('test'){
        steps {
          echo "test step"
          sh 'mvn test'
        }
      }
        stage('deploy'){
        steps {
        echo "deploy step"
        sshPublisher(publishers: [sshPublisherDesc(configName: 'Docker-host', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker rmi -f may13:v.1; docker build -t may13:v.1 .; docker tag may13:v.1 joelarnaud/may13:v.1; docker push joelarnaud/may13:v.1', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: '**/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false), sshPublisherDesc(configName: 'Ansible-host', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook /etc/ansible/may13.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            deploy adapters: [tomcat8(credentialsId: 'tomcatID', path: '', url: 'http://35.183.72.84:8080/')], contextPath: null, onFailure: false, war: '**/*.war'
        }
      }
  }
 }
