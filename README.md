# Actions SDK Java Fulfillment Library

![CI Statuss](https://github.com/jlengrand/assistant-conversation-java/workflows/Java%20CI%20with%20Maven/badge.svg)
![Latest Release](https://img.shields.io/github/v/release/jlengrand/assistant-conversation-java)

This library exposes a developer friendly way to fulfill Actions SDK handlers for the Google Assistant using Java. The Java classes are generated based on [actions-on-google/assistant-conversation-schema](https://raw.githubusercontent.com/actions-on-google/assistant-conversation-schema).

The latest [actions on SDK](https://developers.google.com/assistant/conversational/webhooks#external_https_endpoint) documentation from Google is currently heavily skewed towards Typescript and Firebase. I created this jar simply because I want to be able to create my fulfillments locally using the JVM.

You can use that library to create your own voice bots. The documentation can be found [here](https://developers.google.com/assistant).

_Note: you might not see much activity on this repository, simply because Google seems to not update [their schema](https://raw.githubusercontent.com/actions-on-google/assistant-conversation-schema) very often. It's active, don't worry!_


## What

This repository simply takes the latest version of the [JSON Schema](http://json-schema.org/) 6 file with types to fulfill Actions SDK handlers for the Google Assistant and generates POJOs from it. You can find the file [here](https://github.com/actions-on-google/assistant-conversation-schema/blob/master/conversation.json).
To generate these files, I use [jsonschema2pojo](https://github.com/joelittlejohn/jsonschema2pojo) 's Maven plugin.


## Compilation

This is not something you need per se, but here is how it's done : 

```bash
$ git clone git@github.com:jlengrand/assistant-conversation-java.git
$ cd assistant-conversation-java
$ mvn package
```

You'll find a .jar file in the `target` folder at the end of the compilation

`Note : You'll need Java 8. jsonschema2pojo generates Java level 6 POJOS, so YMMV if you try to compile with another Java version`.

## Usage

### Downloading the dependency

A simple and quick way to access the dependency is to download the latest release. **[You can find it here](https://github.com/jlengrand/assistant-conversation-java/releases)**.

Add it as a dependency to your project and you're good to go!

### Importing the dependency using Maven or Gradle

Whether you use `Gradle` or `Maven`, you want to download **[the dependency](https://github.com/jlengrand/assistant-conversation-java/packages/344243)**.

For example with Maven, add the following to your pom.xml :

```
<dependency>
  <groupId>assistant.conversation.schema</groupId>
  <artifactId>assistant-conversation-java</artifactId>
  <version>1.0</version>
</dependency>
```

Note that since this package is not on the Maven central repository, you will have to add some configuration to your Maven or Gradle settings. 
You can read the complete documentation about how to import Github Packages [here for Maven](https://docs.github.com/en/packages/using-github-packages-with-your-projects-ecosystem/configuring-apache-maven-for-use-with-github-packages), or [here for Gradle](https://docs.github.com/en/packages/using-github-packages-with-your-projects-ecosystem/configuring-gradle-for-use-with-github-packages).

In essence, you want to add the Maven repository to your `settings.xml` file.

```
 <repository>
  <id>github</id>
  <name>GitHub OWNER Apache Maven Packages</name>
  <url>https://maven.pkg.github.com/jlengrand/assistant-conversation-java</url>
</repository>
```

_Note: For gradle, [this link](https://docs.gradle.org/current/userguide/declaring_repositories.html) was very useful for me._

### Using the dependency

Once imported, you are ready to use the POJOs in your class. In short, 

* Create a HTTP Server available form the internet **behind HTTPS**.
* In your Actions app, define an external webhook as fulfillment method and point it to your server. [Here is the documentation](https://developers.google.com/assistant/conversational/webhooks#external_https_endpoint). 
* In your web server, you will receive `HandlerRequest` objects, and will have to return `HandlerResponse`.

Here is a working snippet of a Kotlin Spring boot based app (only the relevant part is shown)

```kotlin
package nl.lengrand.conversations

import assistant.conversation.schema.HandlerRequest
import assistant.conversation.schema.HandlerResponse
import assistant.conversation.schema.Prompt
import assistant.conversation.schema.Simple
import org.springframework.web.bind.annotation.*

@RestController
class FulfillmentController {

    @PostMapping("/fulfillment")
    fun fulfillment(@RequestBody handlerRequest: HandlerRequest) : HandlerResponse {
        val simple = Simple()
        simple.text = "Fun stuff!"
        simple.speech = "Fun stuff!"

        val prompt = Prompt()
        prompt.firstSimple = simple

        val handlerResponse = HandlerResponse()
        handlerResponse.session = handlerRequest?.session;
        handlerResponse.scene = handlerRequest?.scene;
        handlerResponse.prompt = prompt

        return handlerResponse;
    }
}
```

In that case, if my server is available on `https://lengrand.dev`, I want to point thte actions project to `https://lengrand.dev/fulfillment`

## Improvements

* Currently, as you can see in the snippet I use POJOs with Getters and Setters. This works but is surely not idiomatic. It would be interesting to create a [DSL](https://en.wikipedia.org/wiki/Domain-specific_language) to ease creating responses.
* Generation of new versions is done manually using Git tags. If Google releases many new versions, this will need to be automated. 

## Author

* [Julien Lengrand-Lambert](https://github.com/jlengrand) though I only created the jar file.

## License
See [LICENSE](LICENSE).

