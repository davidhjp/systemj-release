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

// Once buildscript dependencies are added, a CompileSystemJ task can be created
task SystemJ(type: com.systemj.CompileSystemJ) {
	//  Libraries needed to compile SystemJ programs
	classpath buildscript.configurations.classpath

	// OPTIONAL: Adding options explicitly, value: keys.
	//           Give an empty string for the non-valued options (e.g. --silence). 
	// options "--config-gen": "default.xml", "--silence": "" 

	// OPTIONAL: Adding directories containing .sysj files, default: src/main/systemj
	//           sysj files are recursively searched and compiled 
	// target files('path-to-dir')

	dependsOn compileJava
}

task runApp(type: JavaExec) {
	// Adding required Java classpaths
	classpath 'build/classes/main', buildscript.configurations.classpath

	// This is the entry point of the SystemJ runtime to run the program.
	main = 'com.systemj.SystemJRunner'

	// A configuration file is loaded from src/main/configurations/default.xml
	args 'src/main/configurations/default.xml'
}
```