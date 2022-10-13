---
title:  "Reusable Clean Architecture"
date: 2022-10-11 20:20:00
categories: software development
---
The following package structure is my personal preferred project structure.

Beside the fact to use a sustainable clean architecture, the main goal of the following example structure is: To have a project, which allows a team to reuse the technical setup.
This is achievable by the separation into modules and commons. Just replace the modules topics with new topics and reuse the commons topics.

<div class="post-image">
  <a href="/assets/images/2022-10-14.svg" target="_blank">
    <img src="/assets/images/2022-10-14.svg"/>
  </a>
  <p>Source: Matthias Achtelik</p>
</div>
<!--more-->

modules & commons
* modules = Package for business use cases which are unique for each application.
* commons = Package for technical use cases which are reusable in each application.

topics
* topics = Packages to split up code into specific use cases. Topics can have multiple levels, see notifications, mails and pushes.
  * business topic = part of the modules package
  * technical topic = part of shares, because you can reuse it in the next application

commons
* commons is a package to centralize code, which is used by multiple topics. It can contains all types of layers.

layers
* layers are for the code separation into clear responsibilities.
  * entrypoints = These are the start points for your process inside the applications. We separate them by technologies.
    * rest = For REST-API endpoints.
    * job = For schedulers which are triggers processes by time.
  * ... = Other technologies could be jms, soap, or other stuff.
  * applications = This layers links the entrypoints with the domains layers and the domains layer with itself. It allows to add additional framework features like dependency injection, transaction management and caching.
    * configs = Contains configuration and property classes or other framework specific stuff.
    * services = Applications services implements the domains services and supports dependency injection. As alternative you also can use in Spring @Bean annotation inside a configs configuration.
  * domains = This contains your main business logic. Have to be independent from other layers and should be as much as possible framework agnostic.
    * adapters = Interfaces which have to be implemented by dataproviders.
    * models = Your business objects which represents your data.
    * services = The main part for your business logic.
  * dataproviders = This are the outgoing points for your process/application to call external systems. We separate them by technologies.
    * db = To store and load data from a database.
    * rest = To make external REST http calls to other systems.
    * ... = Other technologies could be jms, file, or other stuff.

tests
* test is a package which contains additional classes to simplify your tests. For example:
  * testdata = Builders and factories to create test data.
  * base = Classes which create the basic setup for your tests. For example to:
    * Setup a database and other test dependencies.
    * Create a web context.
    * ...
  * archUnit = I really like https://www.archunit.org/. It helps to test architecture decisions. It allows you to:
    * Check your package structure.
    * Check for dependencies cycles.
    * Check naming conventions.