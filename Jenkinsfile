pipeline{

  agent any

    tools{

      maven 'maven3.6.0'

      jdk 'java1.8.0'

    }

    stages{

        //stage('Build'){

              //steps{

                  //sh "mvn -B -DskipTests clean package"

              //  }

           // }

       // stage('Test'){

             // steps{

                  //sh "mvn test"

             // }
//
               // post{
//
                     // always{

                            //junit 'target/surefire-reports/*.xml'

                          //}

                    // }

               // }
          stage('Clone'){
            steps{
              git branch: 'master', url: "https://github.com/Guruprasadmhl/simple-java-maven-app.git"
               }
           }
            
           stage('Artifactory configuration'){
              steps{
                rtServer(
                  id: "ARTIFACTORY_SERVICE",
                    url: "http://34.214.222.45:8081/artifactory",
                    credentialsId: "artifactory_admin"
                )
                rtMavenDeployer(
                                id: "MAVEN_DEPLOYER",
                                serverId: "ARTIFACTORY_SERVICE",
                                releaseRepo:"libs-release-local",
                                snapshotRepo:"libs-snapshot-local"
                  )
                rtMavenResolver(
                    id: "MAVEN_RESOLVER",
                    serverId: "ARTIFACTORY_SERVICE",
                    releaseRepo:"libs-release",
                    snapshotRepo:"libs-snapshot"
                  )
                }
              }
             stage('Exec Maven'){
               steps{
                 rtMavenRun(
                   tool:"maven3.6.0",
                    pom:'pom.xml',
                     goals:'clean install',
                     deployerId:"MAVEN_DEPLOYER",
                     resolverId:"MAVEN_RESOLVER"
                   )
                 }
               }
                   
                   stage('publish build info'){
                     steps{
                       rtPublishBuildInfo(
                         serverId: "ARTIFACTORY_SERVICE"
                      )
                     }
                   }
                      

     // stage ('Deploy'){

           // steps {

              //  sh 'scp -o StrictHostKeyChecking=no -i /tmp/my.pem target/my-app-1.0-SNAPSHOT.jar centos@54.202.246.243:/tmp/'
               // sh 'ssh -i /tmp/my.pem centos@54.202.246.243 java -jar /tmp/my-app-1.0-SNAPSHOT.jar'
                

            //  }

     // }

  }

}
