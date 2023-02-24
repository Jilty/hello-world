// def secrets =  [
//  [ path: 'secrets/bfsi', 
//  engineVersion: 1, 
//  secretValues: [
//   [  envVar: 'orgId', vaultKey: 'orgId'],
//   [envVar: 'username', vaultKey: 'username']
//  ]
// ]]
// def configuration =[vaultUrl: 'http://128.199.253.112:8200',vaultCredentialId: 'vault-jenkins-role',engineVersion: 1]


pipeline
{
 agent any
 	tools { 
   nodejs "node" 
   maven "maven"
   jdk "java1.8"
   }

 stages{
  
  
  stage('Vault'){
   steps{
script{
 
   node(defaultNodeLabel()){
    def my-secret= "test"
    sh 'echo $my-secret'
     withCredentials([[
            $class: 'VaultTokenCredentialBinding',
            credentialsId: 'vault-jenkins-role',
            vaultAddr: 'http://128.199.253.112:8200']]) {
            sh 'vault kv get secrets/bfsi'
        }
    
        sh 'echo $my-secret'


        def secrets = [
            [path: 'secrets/bfsi', secretValues: [
                [vaultKey: 'orgId'],
                [vaultKey: 'username']]]
        ]
//         withVault([vaultSecrets: secrets]) {
//             sh 'echo ${env.orgId}'
//             sh 'echo ${env.username}'
//         }
    }
//  withVault([configuration:[configuration: configuration, vaultSecrets: secrets){
//      withVault([configuration:[vaultUrl: 'http://128.199.253.112:8200',vaultCredentialId: 'vault-jenkins-role',engineVersion: 1], vaultSecrets: [[path: 'secrets/bfsi', engineVersion: 1, secretValues: [ [envVar: 'orgId', vaultKey: 'orgId'],[envVar: 'username', vaultKey: 'username']]]]]){

//   LAST_STARTED = env.STAGE_NAME
//      sh 'echo $orgId'
//        sh 'echo $username'
 // }
}
  }
  }
 
  stage('Build Application'){
   steps{
script{
    configFileProvider([configFile(fileId: '16fec727-a2d6-47dd-94f6-35d0c5e82602', variable: 'settings')]){
  LAST_STARTED = env.STAGE_NAME
sh 'mvn -f pom.xml -s $settings clean install -DskipTests'
  }
}
  }
  }
  



// stage('Munit & Functional Testing'){
//         steps {
// script {
// configFileProvider([configFile(fileId: '16fec727-a2d6-47dd-94f6-35d0c5e82602', variable: 'settings')]){
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
    configFileProvider([configFile(fileId: '16fec727-a2d6-47dd-94f6-35d0c5e82602', variable: 'settings')]){
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
