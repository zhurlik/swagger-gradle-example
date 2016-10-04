# swagger-gradle-example
A very easy way how to use swagger-maven-plugin in gradle projects.
## How to execute swagger-maven-plugin
You can use Groovy to specify settings for swagger-maven-plugin
```groovy
    final ApiDocumentMojo mavenTask = Class.forName('com.github.kongchen.swagger.docgen.mavenplugin.ApiDocumentMojo',true, customClass).newInstance(
            apiSources: [
                    new ApiSource(
                            springmvc: false,
                            locations: ['com/github/kongchen/swagger/sample/wordnik/resource'],
                            schemes: ['http', 'https'],
                            host: 'petstore.swagger.wordnik.com',
                            basePath: '/api',
                            info: new Info(
                                    title: 'Swagger Maven Plugin Sample',
                                    version: 'v1',
                                    description: 'This is a sample for swagger-maven-plugin',
                                    termsOfService: 'http://www.github.com/kongchen/swagger-maven-plugin',
                                    contact: new Contact(
                                            email: 'kongchen@gmail.com',
                                            name: 'Kong Chen',
                                            url: 'http://kongch.com'
                                    ),
                                    license: new License(
                                            url: 'http://www.apache.org/licenses/LICENSE-2.0.html',
                                            name: 'Apache 2.0'
                                    )
                            ),
                            outputPath: file("${buildDir}/swagger/document.html").path,
                            swaggerDirectory: file("${buildDir}/swagger/swagger-ui").path,
                            templatePath: file("${project(':swagger-maven-example').projectDir}/templates/strapdown.html.hbs")
                    )
            ]
    )

    mavenTask.execute()
```
However there is a small trick for adding into classpath all required libs and compiled classes which will be used by swagger-maven-plagin.    
The following code shows how to add swagger-maven-plugin to the gradle script and how to add a reference to classes to be scanned by swagger plugin.
```groovy
buildscript {
    repositories {
        mavenCentral()
    }

    dependencies {
        classpath files(project(':swagger-maven-example').configurations['runtime'].files)
        classpath files(project(':swagger-maven-example').sourceSets['main'].output.classesDir)
    }
}
```

