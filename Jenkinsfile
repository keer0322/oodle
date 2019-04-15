env.dockerimagename="buildon/buildon:v2"
def jupiter_host='34.230.167.197'
def talend_cmd_host='34.230.167.197'
def talend_cmd_user='admin@company.com'
def code_path="${WORKSPACE}/OODLE"
node {
   stage ('Code Checkout') {
    checkout scm
     echo "Listing the files in the current dir"
       sh """ls -ltr
       pwd
       """
    
  }
   stage ('Code compile') {
    sh """pwd
          cd "${WORKSPACE}/OODLE"
          mvn org.talend:ci.builder:6.3.1:generate -f pom.xml -Dcommandline.workspace="${code_path}/" -Dcommandline.host=${talend_cmd_host} -Dcommandline.port=8002 -Dcommandline.user=${talend_cmd_user}
          ls -ltr
          """
    sh 'sleep 10s'
   }
 stage ('Code Build') {
    sh """pwd
          ls -ltr
          cd "${WORKSPACE}/"
          mvn package -f pom.xml -Dcommandline.workspace="${code_path}/" -Dcommandline.host=${talend_cmd_host} -Dcommandline.port=8002 -Dcommandline.user=${talend_cmd_user} -DprojectsTargetDirectory="${WORKSPACE}/target"
          """
    sh 'sleep 10s'
   }
   stage ('Code Deploy') {
    echo "Deploying code"
    sh"""
      cd "${code_path}/target/"
      ls -ltr
      pwd
      set +e
      mvn deploy -fn -e -s settings.xml
      set +e
    """
   }
      stage ('Executing the tests') {
         echo "Executing the tests"
    sh"""
      set +e
      #token=$(curl -H "Content-Type:application/json" -X POST http://10.223.64.51:8191/jupiter/api/getLoginDatas -d '{"username":"user","password":"Password1!","userType":"jupiter"}'>&1)
      #result=$(curl -s -o /dev/null -w "%{http_code}\n" http://10.223.64.51:8191/jupiter/api/executeTestWithStatus -H "Authorization:$token" -H "Content-Type:application/json" -X POST -d '{"projectId":"1000","releaseId":"1","environmentName":"QA","featureFiles":["employee.feature"]}' >&1)
      #if [ "$result" != "200" ]
      #then
      #   exit 1
      #fi
      set +e
    """
   }
}
