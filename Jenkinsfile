pipeline
{
 agent any
 	tools { nodejs "node" }

 stages{
 
  stage('Build Application'){
   steps{
script{
    configFileProvider([configFile(fileId: 'da01fc76-5c2b-4f0d-948a-c101b4cc4340', variable: 'settings')]){
  LAST_STARTED = env.STAGE_NAME
sh 'mvn -f pom.xml -s $settings clean install -DskipTests'
  }
}
  }
  }


// stage('Munit & Functional Testing'){
//         steps {
// script {
// configFileProvider([configFile(fileId: 'da01fc76-5c2b-4f0d-948a-c101b4cc4340', variable: 'settings')]){
// LAST_STARTED = env.STAGE_NAME
// sh "mvn -f pom.xml -s $settings -Dhttp.port=8082 -Dproject.version=1.0.0 -Dsecure.key=mule -Dmule.env=dev -Dtestfile=src/test/javarunner.TestRunner.java test "
// publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site/munit/coverage', reportFiles: 'summary.html', reportName: 'Munit coverage Report', reportTitles: ''])
// cucumber(failedFeaturesNumber: -1, failedScenariosNumber: -1, failedStepsNumber: -1, fileIncludePattern: 'cucumber.json', jsonReportDirectory: 'target', pendingStepsNumber: -1, skippedStepsNumber: -1, sortingMethod: 'ALPHABETICAL', undefinedStepsNumber: -1)
// }
// }
//               }  
//      }
 
 


   
 
  stage('Deploy application to cloudHub'){
   steps{
    configFileProvider([configFile(fileId: 'da01fc76-5c2b-4f0d-948a-c101b4cc4340', variable: 'settings')]){
  sh 'mvn -f pom.xml -s $settings deploy -DmuleDeploy -DskipTests -Dusername=jilty -Dpassword=Jilty@123 -DapplicationName=hello-world-dev-in -Dap.client_id=8c0ff4f9c24149269e6160339bd985b5  -Dap.client_secret=b6410B746D2647768747CA4c17eE64D3 -Dapp.runtime.server=4.4.0 -Ddeployment.env=dev-in  -Dsecure.key=mule -Dworkers=1 -DworkerType=null -Danypoint.businessGroup="NJC India"'


  }
   }
  }
 
 
 
 
 
  // stage('Kill container') {
  //     steps {
  //       script {
  //         LAST_STARTED = env.STAGE_NAME
  //             sh 'docker rm -f apix'
  //       }
  //     }
  //   }
 
 
  }

// post {
//         failure {
//    script {

// if (container_Up)  {
// sh 'docker rm -f apix'
// }
//    }
//         }
//     }
 }
