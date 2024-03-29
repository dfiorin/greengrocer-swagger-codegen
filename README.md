# greengrocer-swagger-codegen(v1.0.0)
Spring boot + Maven + Swagger Codegen -> Automated codegen multi module project. 

# Module description

Parent:

-boot: Main class, spring boot config (properties per environment), bean config classes, integration test

-core: business classes (Delegate, Service, model obj), unit test

-schema: yaml file with API contract

-api: auto-generated classes

# Technical description

Under schema module there's a yaml file, our contract. Define the operations using swagger specification, input, output, what is mandatory and what is not.
In this yaml there's an operation called "buyApples", a simple POST with input and ouptut definition.
How the process works? 
During maven lifecycle:
1) -schema module bundles yaml file with "maven-remote-resources-plugin".
2) -api module, during initialize phase, adds target/generated-sources/swagger/src/main/java to the classpath
3) -api module, during generate-sources phase, takes the bundle from -schema module and generates classes under target/generated-sources/swagger/src/main/java

# Auto generated classes overview

## io.swagger.api 
1) GreengrocerApi.java -> interface with operation definition (buyApples operation)
2) GreengrocerApiController.java -> generic implementation of api. Annotated with @Controller. It contains an instance variable of the delegate. It implements getDelegate(), 
a method that returns delegate instance variable.
3) GreengrocerApiDelegate.java -> interface with default method that returns "not implemented".

## io.swagger.api.model

This package contains all the auto generated model objects (es: request, response).

# So what?

100% api exposition layer has been autogenerated. All you have to do is start writing your business logic. Have a look at -core module, GreengrocerApiDelegateImpl.java.

# Next releases

- Test driven development example: "buyApples" operation.

# V2.0.0 (in progress)

It is possible to customize codegen for your needs, generating all the classes you desire.
In a microservices architecture, the power of codegen is clear. In the next release I'll customize the codegen, adding some mustache files for:
- REST api clients with injected RestTemplate
- REST api clients with Spring Cloud Feign
- clean model object (without unused import and annotations)


