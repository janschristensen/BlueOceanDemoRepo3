node { 
	//Se more about this example https://www.jfrog.com/video/artifactory-5-jenkins-integration/
    def server = Artifactory.server 'Bussen'
    def rtGradle = Artifactory.newGradleBuild()

    stage 'Build'
        git url: 'https://github.com/jfrogdev/project-examples.git'

    stage 'Artifactory configuration'
        rtGradle.tool = 'Gradle'
        rtGradle.deployer repo:'blue-wagon',  server: server
        rtGradle.resolver repo:'train', server: server

    withEnv(['DONT_COLLECT=FOO']){
        stage 'Config Build Info'
            def buildInfo = Artifactory.newBuildInfo()
            buildInfo.env.capture = true
            buildInfo.env.filter.addInclude("*")
            buildInfo.env.filter.addExclude("DONT_COLLECT*")

        stage 'Extra gradle configurations'
            rtGradle.deployer.artifactDeploymentPatterns.addExclude("*.war")
            rtGradle.usesPlugin = true // Artifactory plugin already defined in build script

        stage 'Exec Gradle'
            rtGradle.run rootDir: "gradle-examples/4/gradle-example/", buildFile: 'build.gradle', tasks: 'clean artifactoryPublish', buildInfo: buildInfo

        stage 'Publish build info'
            server.publishBuildInfo buildInfo
    }
}