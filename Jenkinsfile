pipeline{
	agent { label 'master'}
  stages{
    stage('SCM'){
	    steps{
	    git branch: 'master',
	        url: 'https://github.com/javahometech/multibranch'
	    }
    }
  }
}
