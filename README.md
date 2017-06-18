# OpenSourceExperience
# Java Chassis [![Build Status](https://travis-ci.org/ServiceComb/java-chassis.svg?branch=master)](https://travis-ci.org/ServiceComb/java-chassis?branch=master)[![Coverage Status](https://coveralls.io/repos/github/ServiceComb/java-chassis/badge.svg?branch=master)](https://coveralls.io/github/ServiceComb/java-chassis?branch=master)
ServiceComb Java Chassis is a Software Development Kit (SDK) for rapid development of microservices in Java, providing service registration, service discovery, dynamic routing, and service management features


## Quick Start

Provider service:
```java
import io.servicecomb.*;
@RpcSchema(schemaId = "helloworld")
public class HelloWorldProvider implements HelloWorld {
    public String sayHello(String name) {
        return "Hello " + name;
    }
}
```

Consumer service:
```java
import io.servicecomb.*;
@Component
public class HelloWorldConsumer  {
	@RpcReference(microserviceName = "pojo", schemaId = "helloworld")
	private static HelloWorld helloWorld;

	public static void main(String[] args) {
		helloWorld.sayHello("Tank");
	}
}
```

Provider service:
```java
import io.servicecomb.*;
import javax.ws.rs.*;

@RestSchema(schemaId = "helloworld")
@Path("/helloworld")
@Produces(MediaType.APPLICATION_JSON)
public class HelloWorldProvider implements HelloWorld {
    @Path("/sayHello")
    @GET
    public String sayHello(@Pathparam("name") String name) {
        return "Hello " + name;
    }
}
```

Consumer service:
```java
import io.servicecomb.*;
import org.springframework.*;

@Component
public class HelloWorldConsumer  {
	private static RestTemplate restTemplate = RestTemplateBuilder.create();

	public static void main(String[] args) {
       String result= restTemplate.getForObject("cse://jaxrs/helloworld/syaHello?name={name}",String.class,"Tank");
	}
}
```
Provider service:
```java
import io.servicecomb.*;
import org.springframework.*;

@RestSchema(schemaId = "helloworld")
@RequestMapping(path = "/helloworld",produces = MediaType.APPLICATION_JSON)
public class HelloWorldProvider implements HelloWorld {

    @RequestMapping(path = "/sayHello",method = RequestMethod.GET )
    public String sayHello(@RequestParam("name") String name) {
        return "Hello " + name;
    }
}
```

Consumer service:
```java
import io.servicecomb.*;
import org.springframework.*;

@Component
public class HelloWorldConsumer  {
	private static RestTemplate restTemplate = RestTemplateBuilder.create();

	public static void main(String[] args) {
       String result= restTemplate.getForObject("cse://springmvc/helloworld/syaHello?name={name}",String.class,"Tank");
	}
}
```


## Documentation

Project documentation is available on the [ServiceComb website][servicecomb-website].

[servicecomb-website]: http://servicecomb.io/


## Building

You don’t need to build from source to use Java Chassis (binaries in repo.servicecomb.io), but if you want to try out the latest and greatest, Java Chassis can be easily built with the maven.  You also need JDK 1.8.

      mvn clean install

The first build may take a longer than expected as Maven downloads all the dependencies.

## Automated Testing

  To build the docker image and run the integration tests with docker, you can use maven docker profile 
  
      mvn clean install -Pdocker
      
  If you are using docker machine, please use the following command
  
      mvn clean install -Pdocker -Pdocker-machine
            
## Contact

Bugs: [issues](https://github.com/servicecomb/service-center/issues)

## Contributing

See CONTRIBUTING for details on submitting patches and the contribution workflow.

## Reporting Issues

See reporting bugs for details about reporting any issues.
