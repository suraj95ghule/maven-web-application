pipeline
{
    agent
      {
          label
            {
                label "maven1"
                customWorkspace "/opt/project"
            }
      }
    options
      {
          timeout(time: 5, unit: 'MINUTES')
      }
    parameters
      {
          booleanParam(name: 'skip_deploy', defaultValue: false, description: 'Set to true to skip the test stage')
      }
     stages
      {
          stage("init")
            {
                steps
                  {
                      sh "rm -fr *"
                  }
            }
            stage("clone")
              {
                  steps
                   {
                       sh "git clone -b feature/test-suraj https://github.com/suraj95ghule/maven-web-application.git"
                   }
              }
            stage("build")
             {
                 
                 steps
                   {
                       catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE')
                       {
                       sh " cd /opt/project/maven-web-application && mvn clean install"
                       //sh "exit 1"
                       
                    }
                   }
             }
             stage("deploy")
               {
                   when { expression { ( params.skip_deploy != true) } }
                   steps
                     {
                         catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE')
                         {
                         sh "cp /opt/project/maven-web-application/target/*.war /opt/tomcat/apache-tomcat-9.0.100/webapps"
                     }
                     }
               }
      }
     
}
