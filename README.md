# Actions SDK Java Fulfillment Library

This library exposes a developer friendly way to fulfill Actions SDK handlers for the Google Assistant using Java. The Java classes are generated based on [actions-on-google/assistant-conversation-schema](https://raw.githubusercontent.com/actions-on-google/assistant-conversation-schema).

The latest [actions on SDK](https://developers.google.com/assistant/conversational/webhooks#external_https_endpoint) documentation from Google is currently heavily skewed towards Typescript and Firebase. I created this jar simply because I want to be able to create my fulfillments locally using the JVM.

You can use that library to create your own voice bots. The documentation can be found [here](https://developers.google.com/assistant).

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


TODO


## Author

* [Julien Lengrand-Lambert](https://github.com/jlengrand) though I only created the jar file.

## License
See [LICENSE](LICENSE).

