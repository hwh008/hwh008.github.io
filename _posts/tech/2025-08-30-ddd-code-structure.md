---
layout: post
title: 'DDD structure improve'
date: 2025-08-30
author: huang
tags: ddd
---

Skip my last [DDD post](https://hwh008.github.io/2024/09/29/ddd-code-structure.html). I improve on it again, the DDD practice method.


A business domain package includes a group of cohesive classes.

### 1. model

I create a sub-directory named “model” to hold the model classes (like POJO in Java) and related model-utility classes, e.g., User.class and UserUtil.class. Utility classes are merely for Java and they introduce business and database operation functions.

### 2. util

An “util” sub-directory contains utility classes and primitive classes. Primitive classes are immutable. E.g., StrUtil.class and Address.class.

### 3. gateway

The “gateway” sub-directory houses anti-corruption interfaces and their implementation classes, and also has API client classes that have a one - to - one correspondence with API functions. E.g., PaymentService.class and SomeVendorPaymentClient.class.

### 4. dto

All DTO classes are loaded in a “dto” sub-directory.

### 5. app-service

The root directory of the package hosts usecase service classes that serve relevant specific business processes.


And there is a package module class that retains and initializes the service instances of this package, and exports them for other packages to call.


For instance, a “team” package might have the TeamMod.class with an init function and a teamChatService instance.
I introduce the Mod.class to avoid excessive binding with a specific DI framework.

### 6. framework and infrastructure

The “app” package contains framework-related codes, and the structure adheres to the framework's conventional and best practices.