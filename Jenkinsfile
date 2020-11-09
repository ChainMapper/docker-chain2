def withPod(body) {
  podTemplate(cloud: 'chainmapper', containers: [
      containerTemplate(name: 'kaniko', image: 'gcr.io/kaniko-project/executor:debug', command: '/busybox/cat', ttyEnabled: true),
      containerTemplate(name: 'kubectl', image: 'joshendriks/kubectl:debug-v1.18', command: 'cat', ttyEnabled: true),
      containerTemplate(name: 'git', image: 'alpine/git', command: 'cat', ttyEnabled: true)
    ],
    volumes: [
	  secretVolume(secretName: 'netwatwezoeken', mountPath: '/cred')
    ]
 ) { body() }
}

def checkoutOrCreateBranch(branch) {
    try {
        sh("git checkout ${branch}")
    } catch (Exception e) {
        sh("git branch ${branch}")
        sh("git checkout ${branch}")
    }
}

withPod {
	node(POD_LABEL) {
		def tag = "${env.BRANCH_NAME.replaceAll('/', '-')}-${env.BUILD_NUMBER}"
		if (env.BRANCH_NAME == 'master' && env.TAG_NAME ) {
            tag = env.TAG_NAME
        } else if (env.BRANCH_NAME == 'master') {
            tag = "latest"
        }
		echo "${tag}"
		//def registry = "packages.netwatwezoeken.nl/chainmapper"
		//def appname = "chain2"
		//def service = "${registry}/${appname}:${tag}"
		//checkout scm		
        //container('kaniko') {
        //    stage('Build image') {
        //        sh("cp /cred/.dockerconfigjson /kaniko/.docker/config.json")
        //        sh("executor --context=`pwd` --dockerfile=`pwd`/Dockerfile --destination=${service} --single-snapshot")
		//	}	
		//}		
	}
}