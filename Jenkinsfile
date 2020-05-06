
def cause = 'UNKNOWN'
def runType = 'UNKNOWN'
def currentDateTime = 'UNKNOWN'

pipeline
{
  agent none
  stages
  {
    stage('Job Causes')
    {
      agent any
      steps
      {
        script
        {
          // Getting job causes
          def causes = currentBuild.getBuildCauses()
          for(causeJSON in causes)
          {
            if (causeJSON.class.toString().contains("UpstreamCause"))
            {
              runType = "Dependency run"
              println "This job was caused by job " + causeJSON.upstreamProject
              cause = "Job cause : " + causeJSON.shortDescription.toString()
            }
            else if (causeJSON.shortDescription.toString().contains("timer"))
            {
              runType = "Nightly run"
              //println "Job cause : " + causeJSON.shortDescription.toString()
              cause = "Job cause : " + causeJSON.shortDescription.toString()
            }
            else
            {
              runType = "Manual run"
              cause = "Job cause : " + causeJSON.shortDescription.toString()
            }
          }
        }
      }
    }
    stage('Getting time')
    {
      agent any
      steps
      {
        script
        {
          currentDateTime = sh ( script: 'date +%d.%m.%y-%H:%M',
                                 returnStdout: true
                               ).trim()
        }
      }
    }
  }
  post
  {
    always
    {
      echo 'pipeline done'
      echo "${cause}"
    }
    success
    {
      echo 'success'
    }
    failure
    {
      echo 'failure'
    }
    aborted
    {
      echo 'Aborted'
    }
  }
}

