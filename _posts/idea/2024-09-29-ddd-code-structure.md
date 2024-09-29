---
layout: post
title: ‘DDD code structure'
date: 2024-09-29
author: huang
cover: 'https://loremflickr.com/320/100/code'
tags: ddd
---

DDD stands for Domain Driven Design/Development. I have done extensive research on DDD and put in a lot of effort to practice it in real business projects. in this article I share my insight of how to organize code with DDD taste.

### 1. Modules in Project

Before delving into the domain structure, it is really important to separate a project into various distinct modules instead of having everything in a jumbled mess. This is similar to vertical division rather than layer division. It's like building a well-organized house where each room (module) has its specific purpose and is clearly bordering.


 In my case, I use modules such as App, Business, External, and Antiseptic.

### 2. Layers in App Module

The App module represents the application framework. It is a technical module that contains all the specific framework codes and tasks. For instance, Spring Boot provides a web server, configuration system, controller system, and schedule system. Your application runs on this framework and requires some setup code.



I set up the framework code in the App module and separate it into several layers. These include:

* Starter layer: Consists of the main class or framework entry class and configuration setup.
 
* Controller layer: Contains all controller classes for HTTP calling. The controller is responsible for converting between DTO (Data Transfer Object) objects from/to HTTP requests and responses.

* TCP layer: Includes TCP server class and handler class which, similar to the controller, is used for converting DTO objects.

* Schedule layer: Contains all cron job classes that can be executed by the framework on time.



I must note that controllers, TCP handlers, and cron jobs are input portals. They only perform codec and then delegate the actual job to business services.

### 3. Layers in External

The External module wraps code for calling other platform APIs and serves communication. For example, when integrating PayPal into a project, we can write the API calling code in this module.



I create layers and specific classes in this spot:

* Service layer: Includes client classes which are equivalent to every API calling; server service classes which are equivalent to every API that should be exposed to a particular external entity.



We use client classes in the Business module (actually used in Antiseptic). However, we serve to the external only in this specific spot.


* DTO layer: Holds all the API protocol objects from request to response. When communicating with an external API like PayPal, the data we send and receive needs to be in a specific format. The DTO layer defines these formats.



Typically, I structure the layers in External like this. But if more complex server services are required, I still put domain layers and converter layers in it. I will explain these layers in the next module.

### 4. Layers in Business

Business is involved with the core business operations. Although it is often thought of as basic CRUD operations, in reality, it should encapsulate operation rules and processes rather than just being limited to CRUD.

A common operation process has the following steps:

* Convert parameter DTO (Data Transfer Object) to domain object.

* Delegate to the domain object function.

* Call external API.

* Proceed to various branches based on conditions.

* Convert the domain object to response DTO object.

I set up most of the prime domain layers in this module.

* Model layer: I write all domain model classes in this layer. The model business function names start with “doSomething”. I also put model manager classes in this layer. The manager is a collection owner that confine specific rules on the models within the collection.
* Types layer: It includes the unchangeable domain object classes which are named as value objects in DDD. Types are important objects like models and are used for encapsulating business rules rather than placing this code in a utility class.


for example Money class is composed of a number and currency type, and has the rules to generate a number for local use.

* Repo layer: It contains DAO (Data Access Object) classes to load and save model objects. It is the bridge between objects and the database.

* Service layer: As mentioned earlier, it involves classes that assign business processes to model objects. It encapsulates various business use cases. Service classes wrap functions based on operator roles. For example, an admin role and a user role respectively have their own service classes with different actions.

an admin role might have a "AdminService" class that contains functions like "approveUserRegistration" to approve new user registrations; A user role, on the other hand, might have a "UserService" class with functions like "viewProfile".

* DTO layer: Includes the DTO object classes used in service classes.

* Convert layer: It contains converter classes to convert objects between DTO objects and domain objects.

For example, when a new order is created from a user's input (DTO object), the convert layer will convert it into a domain object that can be processed by the business layer. Similarly, when the result of a business operation (domain object) needs to be sent back as a response to the user, the convert layer will convert it back into a DTO object

* Antiseptic layer: I write antiseptic interfaces that are used in service classes in this place. The antiseptic layer acts as a protective layer, ensuring that the business services interact with external systems without acturly dependence.

* Util layer: I put all unsorted classes in this layer. It's like a super pocket.

### 5. Layers in Antiseptic

Antiseptic consists of classes that cover all calls to external services. However, it is designed to adapt to business domain operations and hides all the actual external client class APIs. like a shield that protects the internal workings of the application from the complexity of external services.

* Service layer: The main layer in this module is the service layer which includes implementation classes derived from the Business antiseptic interface. they hide the dirty work.

* Convert layer: Yes, there should be a place to store codes for converting objects between domain objects and external client DTO objects.

* Repo layer: Sometimes I have to cache the DTO object for later reuse. it's really usefull to maintain the internal concise of Business services.
