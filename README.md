# greeting-rest-service

 [![status](https://travis-ci.org/cppdk/greeting-rest-service.svg?branch=master)](https://travis-ci.org/cppdk/greeting-rest-service) 
 [![coverage](https://codecov.io/gh/cppdk/greeting-rest-service/coverage.svg?branch=master)](https://codecov.io/gh/cppdk/greeting-rest-service)
 [![Technical Debt](https://sonarcloud.io/api/badges/measure?key=com.example%3Agreeting-rest-service&metric=sqale_debt_ratio)](https://sonarcloud.io/dashboard?id=com.example%3Agreeting-rest-service)

## Introduction

A simple rest service that is developed towards being able to greet you in a 
number of languages. Currently it will only greet you with a "Hallo" which is
the Danish, Swedish or Norwegian way to say "Hello". Currently it is perceived
as a Danish greeting and the only supported greetings are Danish and English.

## Worth Noticing

The initial resource "/greetings" is now only supported using a 
  
    application/json

content-type, where the more semantical correct "/greetings" is supported for
a period of time for compliancy by setting the Accept header to 

    application/hal+json

This is mainly done to show how coexistence thus moving along is possible. 
Furthermore the resource "/greetings/{greeting}" is now made with the content
version scheme using "application/hal+json" as producer content-type and 
showing how to use the actual version of content. 

    application/hal+json;concept=greeting;v=1

if a consumer has the need to go back to that version, once a version 2 e.g. 

    application/hal+json;concept=greeting;v=2

is created in a near future.

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

    mvn clean package exec:java@api-docs

To run the REST Server standalone:

    mvn exec:java@start-server 

To test the REST service use e.g. Postman:
    
    GET http://localhost:8080/greetings

    and get a response 200 OK back, with 
     - Headers 
          content-length: 21
          content-type: application/json
     - Body:
            { "greeting": "Hallo!" }

To test the REST service greeting in English:

    GET http://localhost:8080/greetings
    having set Accept-Language "en"

    and get a response 200 OK back, with 
     - Headers 
          content-length: 21
          content-type: application/json
     - Body:
            { "greeting": "Hello!" }


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
            X-RateLimit-Reset
        content-length: 458
        content-type: application/hal+json        

    - Body:
            {
                "greetings": {
                    "info": "a list containing current greetings",
                    "_links": {
                        "self": {
                            "href": "/greetings",
                            "type": "application/hal+json;concept=greeetinglist;v=1",
                            "title": "List of Greetings"
                        },
                        "greetings": [
                            {
                                "href": "/greetings/hallo",
                                "title": "Danish Greeting - Hallo"
                            },
                            {
                                "href": "/greetings/hello",
                                "title": "English Greeting - Hello"
                            }
                        ]
                    }
                }
            }


## The uber-jar

An uber-jar is a so-called "fat jar" containing all the transitive dependencies for the service.
The uber-jar is created during the "package" phase of the build
It is used to facilitate an easy portable complete image of a service.
You can run it as a standalone service.  

To run the uber-jar:

    java -jar target/shaded-greeting-rest-service-1.0-SNAPSHOT.jar
