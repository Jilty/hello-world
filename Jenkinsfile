def secrets = [
  [path: 'secrets/bfsi', engineVersion: 2, secretValues: [
    [envVar: 'orgId', vaultKey: 'orgId'],
    [envVar: 'username', vaultKey: 'username'],
    [envVar: 'password', vaultKey: 'password']]],
]
def configuration = [vaultUrl: 'http://128.199.253.112:8200',  vaultCredentialId: 'vault-jenkins-id', engineVersion: 2]


pipeline
{
 agent any
 	tools { 
   nodejs "node" 
   maven "maven"
   }

 stages{   
  
  stage('Vault'){
   steps{
      script{
         withVault([configuration: configuration, vaultSecrets: secrets]) {
            sh "echo $orgId"
          } 
      }
   }
  }
 
  stage('Build Application'){
   steps{
    script{
      configFileProvider([configFile(fileId: 'b0e08ae7-e5db-4166-956b-41ced22fd16e', variable: 'settings')]){
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
     scrtpt{
       configFileProvider([configFile(fileId: 'b0e08ae7-e5db-4166-956b-41ced22fd16e', variable: 'settings')]){
         sh 'mvn --version'
//             sh 'mvn -f pom.xml -s $settings clean install -DskipTests'
//             sh 'mvn -f pom.xml -s $settings clean deploy -DmuleDeploy'
       }
     }
   }
  } 
 
  }
 }
