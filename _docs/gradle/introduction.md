---
title: 1. Introduction 
category: 2. Gradle
order: 1
---
<h2>Contents</h2>
* toc
{:toc}
## What is Gradle
Gradle is an open source build automation tool, that is, a tool that permits developer to automatize tasks like, for example, test execution, code compiling, deployment, and generation of documentation.
It is the most popular build system for the Java Virtual Machine (JVM) and the default for Android.
The peculiarity of Gradle is that it provides its own high-level and declarative build language that permits to write build logic with ease. On this course, we will focus only on the compilation part of Gradle for java applications.

## Installation
You can install gradle in many different ways:
1. Installing <a href="https://www.jetbrains.com/idea/">IntelliJ IDEA</a>: Gradle will be installed by default (**Recommended**).
2. Installing the <a href="https://code.visualstudio.com/docs/java/java-build">Java Extension Pack</a> for <a href="https://code.visualstudio.com/">Visual Studio Code</a>.
3. <a href="https://gradle.org/install/">Manual installation</a> (just follow the documentation).  

For this course, we will use Gradle with IntelliJ IDEA, and we based the lecture for Java.  

**NOTE**: Exists two version of IntelliJ IDEA: Community Edition, that is free, and Ultimate, that is not. The Ultimate version has the support for Spring (that we will see in the next lessons). However, you can obtain a free Student License to use IntelliJ Ultimate (see <a href="https://www.jetbrains.com/community/education/#students"> here </a>).

## Gradle in Action
To see Gradle in action create a new Project with IntelliJ IDEA and choose Gradle as Build System. Set the other other parameters as follows:
![Gradle intro]({{ site.baseurl }}/images/gradle_intro_1.png)
Now open the build.gradle.kts and replace all the content with the next snippet of code (we will explain why in following sections):
{% highlight kotlin %}
plugins {
    id("application")
}

group = "org.example"
version = "1.0-SNAPSHOT"

repositories {
    mavenCentral()
}

dependencies {
    testImplementation(platform("org.junit:junit-bom:5.9.1"))
    testImplementation("org.junit.jupiter:junit-jupiter")
}

application {
    mainClass.set("org.example.Main")
}

tasks.test {
    useJUnitPlatform()
}
{% endhighlight %}
Then, click on the elephant icon highlighted by the arrow in the image below (or, if you don't see it, just reopen the project). This action syncs IntelliJ Gradle plugin with the changes we made.
![Gradle intro]({{ site.baseurl }}/images/gradle_intro_2.png)
Now open the Gradle view (the elephant on the left): here, you have a list of all the gradle tasks that you can launch. A task is a piece of work that Gradle can do with a project. For example, the build task will perform all the steps necessary to get a full build of our java application. Double click on it.
![Gradle intro]({{ site.baseurl }}/images/gradle_intro_3.png)
This will open a run panel on the lower side of the IDE that shows what your action do. You should see something like this:

{% highlight gradle %}
20:22:13: Executing 'build'...

> Task :compileJava
> Task :processResources NO-SOURCE
> Task :classes
> Task :jar
> Task :startScripts
> Task :distTar
> Task :distZip
> Task :assemble
> Task :compileTestJava NO-SOURCE
> Task :processTestResources NO-SOURCE
> Task :testClasses UP-TO-DATE
> Task :test NO-SOURCE
> Task :check UP-TO-DATE
> Task :build

BUILD SUCCESSFUL in 328ms
5 actionable tasks: 5 executed
20:22:14: Execution finished 'build'.
{% endhighlight %}
As you can notice, some tasks were called sequentially, and a new folder appered in your project. If you navigate on it, you can see that it contains different subfolders: classes, distributions, generated, libs and so on. On these folder you get your application in different "format": for example, you get you application as a .jar library, as a set of .class files, or as archives (.zip and .tar) for distribution.  
Now double click on the **run** task, under the application folder:
{% highlight gradle %}
20:49:10: Executing 'run'...

> Task :compileJava UP-TO-DATE
> Task :processResources NO-SOURCE
> Task :classes UP-TO-DATE

> Task :run
Hello and welcome!i = 1
i = 2
i = 3
i = 4
i = 5

BUILD SUCCESSFUL in 163ms
2 actionable tasks: 1 executed, 1 up-to-date
20:49:11: Execution finished 'run'.
{% endhighlight %}
This action will perform all the necessary steps to run the application: as you can see, first it will call the compileJava task (that as the name suggests compile the Java application) and generate .class file. In this case, we did not generated .jar, .zip and .tar files: this because we just wanted to run the application.  

### Command-Line Gradle
A keen eye might have notice that on the root folder of our project there are two scripts files: gradlew and gradlew.bat. These files are wrappers of gradle and permits to launch tasks directly from the terminal. Just try to launch ./gradlew tasks (if you are on macOS / Linux) or gradlew.bat tasks (if you are on Windows). This command will list all the available tasks. You can launch tasks by ./gradlew &lt;task&gt; (or ./gradlew.bat &lt;task&gt;), replacing &lt;task&gt; with the task name. For example:  
![Gradle intro]({{ site.baseurl }}/images/gradle_intro_4.png)  

### A Quick Note About DSL
Maybe you notice that during the creation of the project you could choose the Gradle DSL (Domain Specific Language). This goes to specifies the language that you want to use for your gradle build file (more details on this file later). Kotlin is the preferred choice, since it is more readable and offer better compile-time checks since Kotlin is statically typed. However, syntax differences are minimal, so you can choose whatever you prefer.