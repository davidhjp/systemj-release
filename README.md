# SystemJ Development Kit (SJDK)

You can find the latest releases of the SystemJ development kit
[here](https://github.com/hjparker/systemj-release/releases). Alternatively
you can create a Gradle project as shown below:

```groovy
apply plugin: 'groovy'
defaultTasks 'SystemJ', 'runApp'

buildscript {
	repositories {
		mavenCentral()
		maven {
			url 'https://github.com/hjparker/systemj-release/raw/master/'
		}
	}
	dependencies {
		classpath group: 'com.systemjtechnology', name: 'systemj', version:'v2.1-134'
		classpath group: 'com.systemjtechnology', name: 'systemj', version:'v2.1-134', classifier: 'sjrt-base'
		classpath group: 'com.systemjtechnology', name: 'systemj', version:'v2.1-134', classifier: 'sjrt-desktop'
	}
}

task SystemJ(type: com.systemj.CompileSystemJ) {
//   Libraries needed to compile SystemJ programs
	classpath buildscript.configurations.classpath

//   Adding options explicitly, value: keys
//   options "--config-gen": "default.xml", "--silence": "" // An empty string for non-valued options

//   Adding directories containing .sysj files, default: src/main/systemj
//   .sysj files are recursively searched and compiled
//   target files('path-to-dir')
	dependsOn compileJava
}

task runApp(type: JavaExec) {
	classpath 'build/classes/main', buildscript.configurations.classpath
	main = 'com.systemj.SystemJRunner'
	args 'src/main/configurations/default.xml'
}
```
Run `gradle` to build and run the HelloWorld SystemJ program.