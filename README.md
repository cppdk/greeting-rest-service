# greeting-rest-service

 [![status](https://travis-ci.org/cppdk/greeting-rest-service.svg?branch=master)](https://travis-ci.org/cppdk/greeting-rest-service) 
 [![coverage](https://codecov.io/gh/cppdk/greeting-rest-service/coverage.svg?branch=master)](https://codecov.io/gh/cppdk/greeting-rest-service)
 [![Technical Debt](https://sonarcloud.io/api/badges/measure?key=com.example%3Agreeting-rest-service&metric=sqale_debt_ratio)](https://sonarcloud.io/dashboard?id=com.example%3Agreeting-rest-service)

## Introduction

A simple rest service that is developed towards being able to greet you in a 
number of languages. Currently it will only greet you with a "Hallo" which is
the Danish, Swedish or Norwegian way to say "Hello". Currently it is perceived
as a Danish greeting and the only supported greetings are Danish and English. 
It is possible to add greetings to the service, which means you can create new 
greetings, they will however not be stored, as the purpose of this service is 
to focus on the API. A workshop like edition of the service has been created 
in [github](https://github.com/allanhojgaardjensen/breakfastcoding)



## Worth Noticing

The initial resource "/greetings" is now only supported using either
  
    application/json

as content-type, and

    application/hal+json

Furthermore the resource "/greetings/{greeting}" is now made with the content
version scheme using "application/hal+json" as producer content-type and 
showing how to use the actual version of content. 

    application/hal+json;concept=greeting;v=4

if a consumer has the need to go back to an earlier version 3 e.g. 

    application/hal+json;concept=greeting;v=3

is used as value for the Accept header.

## Working with the service

Underneath there are a collection of useful things to work with using the 
example.

Useful Commands
---------------
To Build this project:

    mvn package

To clean an existing checkout and build:

    mvn clean package

To clean an existing checkout, build and generate API docs:

    mvn clean verify

To run the REST Server standalone:

    mvn exec:java@start-server 

To test the REST service use e.g. Postman:
    
    GET http://localhost:8080/greetings
    Accept application/json or application/hal+json
    and get a list of greetings back

    GET http://localhost:8080/greetings
    having set Accept-Language "en" and
    having set Accept "application/hal+json"
    
    and get a response 200 back with

    - Headers:
        access-control-allow-headers: 
            Content-Type, 
            Authorization, 
            If-Match, 
            If-None-Match, 
            X-Log-Token, 
            X-Client-Version, 
            X-Client-ID, 
            X-Service-Generation, 
            X-Requested-With

        access-control-allow-methods: GET, POST, DELETE, PUT, PATCH, OPTIONS, HEAD
        access-control-allow-origin: *
        access-control-expose-headers: 
            Location, 
            Retry-After,    
            Content-Encoding, 
            ETag, 
            X-Log-Token, 
            X-RateLimit-Limit, 
            X-RateLimit-Limit24h, 
            X-RateLimit-Remaining, 
            X-RateLimit-Reset,
            X-Status
        content-type: application/hal+json
        ...

    - Body:
            {
                "greetings": {
                    "info": "a list containing current greetings",
                    "_links": {
                        "self": {
                            "href": "/greetings",
                            "type": "application/hal+json;concept=greetinglist;v=1",
                            "title": "List of Greetings"
                        },
                        "greetings": [{
                             "href": "greetings/hallo",
                             "title": "Dansk Hilsen Hallo"
                         },{
                             "href": "greetings/hallo",
                             "title": "Danish Greeting Hallo"
                         },{
                             "href": "greetings/hello",
                             "title": "English Greeting Hello"
                         },{
                             "href": "greetings/hello",
                             "title": "Engelsk Hilsen Hello"
                         }]
                    }
                }
            }

To test the REST service greeting in English:

    GET http://localhost:8080/greetings/hello
    having set Accept-Language "en"

    and get a response 200 OK back, 
    with an all English greeting  

    {    
            "greeting": "Hello!",
            "language": "English",
            "country": "England",
        "    native": {
                "language": "English",
                "country": "England"
            },
        "    _links": {
                "self": {
                    "href": "greetings/hello",
                "    title": "English Greeting Hello"
                }
        }
    }
    
To test the REST service greeting in Danish:

    GET http://localhost:8080/greetings/hello
    having set Accept-Language "da"

    and get a response 200 OK back, 
    with an English greeting and part in Danish  
 
    {
        "greeting": "Hello!",
        "language": "English",
        "country": "England",
        "native": {
            "language": "Engelsk",
            "country": "England"
        },
        "_links": {
            "self": {
                "href": "greetings/hello",
                "title": "Engelsk Hilsen Hello"
            }
        }
    }

## The uber-jar

An uber-jar is a so-called "fat jar" containing all the transitive dependencies for the service.
The uber-jar is created during the "package" phase of the build
It is used to facilitate an easy portable complete image of a service.
You can run it as a standalone service.  

To run the uber-jar:

    java -jar target/shaded-greeting-rest-service.jar


## Reports

 * `Checkstyle` for service implementation
 * `Findbugs` for service implementation
 * `PMD` for service implementation
 * `Jacoco Coverage` for service implementation

 * `OpenApi` specification for service API

 * `Maven` Site (if `mvn site` is used)