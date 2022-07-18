#!groovy

@Library('jenkinslib') _

def tools = nwe org.devops.tools()

String workspace = "/data/jenkins/workspace"


pipeline {
    agent { node {  label "master"   //指定运行节点的标签或者名称
                    //customWorkspace "${workspace}"   //指定运行工作目录（可选）
            }
    }

    options {
        timestamps()  //日志会有时间
        skipDefaultCheckout()  //删除隐式checkout scm语句
        disableConcurrentBuilds() //禁止并行
        timeout(time: 1, unit: 'HOURS')  //流水线超时设置1h
    }
    
    parameters { string(name: 'DEPLOY_ENV', defaultValue: 'prod', description: '') }
        
    stages {
    //    stage('Example') {
    //        input {
    //            message "Should we continue?"
    //            ok "Yes, we should."
    //            submitter "zhouyc,chengdh"
    //            parameters {
    //                string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
    //            }
    //        }
    //        steps {
    //            echo "Hello, ${PERSON}, nice to meet you."
    //        }
    //    }
        //下载代码
        stage("GetCode"){ //阶段名称
            when { environment name: 'DEPLOY_ENV', value: 'prod' }
            steps{  //步骤
                timeout(time:5, unit:"MINUTES"){   //步骤超时时间
                    script{ //填写运行代码
                        println("构建环境：${DEPLOY_ENV}")
                        println('获取代码')
                    }
                }
            }
        }
        stage("Build"){ //阶段名称
            steps{  //步骤
                timeout(time:20, unit:"MINUTES"){   //步骤超时时间
                    script{ //项目构建
                        println('项目构建')
                        mvnHome = tool "m2"
                        println(mvnHome)
                        
                        sh "${mvnHome}/bin/mvn --version"
                    }
                }
            }
        }
        stage("CodeScan"){ //阶段名称
            steps{  //步骤
                timeout(time:30, unit:"MINUTES"){   //步骤超时时间
                    script{ //代码扫描 
                        println('代码扫描')
						
						tools.PrintMes("this is my lib")
                    }
                }
            }
        }
    }

    //构建后操作
    post {
        always {
            script{
                println("always")
            }
        }

        success {
            script{
                currentBuild.description = "构建成功!" 
            }
        }

        failure {
            script{
                currentBuild.description = "构建失败!" 
            }
        }

        aborted {
            script{
                currentBuild.description = "构建取消!" 
            }
        }
    }
}
