---
title: Jenkins 使用手冊
date: 2022-12-13 15:32:16
categories: CICD
tags: [Jenkins]
---

## 下載
請參考官網[下載頁面](https://www.jenkins.io/download/)

## 管理任務
將 Jenkins 啟動後，建立一個新的 Job，進入管理頁面(此處使用 Jenkinsfile)
![管理任務(page1)](jenkins-project-detail_page-1.jpg "manage-job-p1")
![管理任務(page2)](jenkins-project-detail_page-2.jpg "manage-job-p2")

## 撰寫 Jenkins File

```
pipeline {
  agent any
  tools { nodejs 'NodeJS' }

  parameters {
    choice(name: 'deploy_env', choices: ['dev', 'sit', 'uat'], description: 'Choose Environment')
  }

  environment {
    build_output = './dist'
    destination_folder = '/Apache24/htdocs/the-project'
  }

  stages {

    stage('Show Info') {
      steps {
        script {
          currentBuild.displayName = "#${currentBuild.number} : ${params.deploy_env}"
        }

        script {
          if (params.deploy_env == 'dev') {
            env.ip_web_server = '192.168.0.28'
          } 
          else if (params.deploy_env == 'sit') {
            env.ip_web_server = '192.168.0.51'
          } 
          else if (params.deploy_env == 'uat') {
            env.ip_web_server = '192.168.0.52'
          }
        }
        sh """
          echo "deploy_env=${params.deploy_env}"
          echo "env.ip_web_server = ${env.ip_web_server}"
          echo "env.WORKSPACE = ${env.WORKSPACE}"
          node -v
          npm --version
        """
      }
    }

    stage('NPM Install') {
      steps {
        echo 'NPM Installing..'
        checkout scm
        sh 'npm install -f'
      }
    }

    stage('Build Project') {
      steps {
        echo 'Building Project...'
        sh """
          npm run build:${params.deploy_env}
        """
      }
    }

    stage('Copy') {
      steps {
        echo 'Copy...'
        sh """
          scp -r ${build_output}/* administrator@${env.ip_web_server}:${destination_folder}
        """
      }
    }

    stage('Notify') {
      steps {
        sh """ 
          curl -X POST https://notify-api.line.me/api/notify \
          -H 'Authorization: Bearer NrYJCadzjvk5agBVzKRPJuxtK0gNuRCTPl8LiglR7v5' \
          -F "message=【${env.JOB_NAME}】測試～"
        """ 
      }
    }
  }
}
```