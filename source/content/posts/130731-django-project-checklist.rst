Checklist for Evaluating Existing Django Projects
=================================================

:category: post
:date: 2013-07-31
:slug: django-project-checklist

Lately I've consulted for several clients where I was asked to drop into
an existing large Django project code base to triage issues and build 
enhancements. Working on an established code base is common whether you're 
a new developer on a team or like me working on consulting projects.

There are many attributes of an existing project: the code (and 
documentation), infrastructure, deployment procedures, and third-party 
services. While reviewing each one of these areas I get either a good feeling
I'll be able to accomplish my work quickly, or a terrible feeling that
the pre-work to triage issues will dwarf the scope of the work requested
by the client.

Here is the checklist I now use (and add to when I uncover something new) 
after having gone through this process of evaluating established projects
many times.

Code base
---------
The code repository is where most developers will dive in and try to
understand the project. A README file that screams "start here!" is a must.

README
~~~~~~
* A brief (one paragraph or bullet points) explanation of what the project 
  is, its purpose, and goals to provide context for developers

* Important links to development wikis, testing, staging, and production
  servers

* Contact information for anyone critical to the project.

* "Getting Started" section that bootstraps developers from a blank
  slate to a functional development environment. Included even if you're
  using Vagrant (to show a new developer how to use Vagrant)
  
* Instructions for special software required for a fully-functioanl site.
  For example, how to set up Solr and build the search indexes, or how to
  locally replicate the database from a test or production environment

* Basic deployment information and/or a link to detailed deployment
  instructions (even if this is just how to use the included Fabric or
  Capistrano scripts)


Project Structure
~~~~~~~~~~~~~~~~~
* Is this a flat 1.3 (and previous versions) Django project structure? 

* Is this a standard Django 1.4+ project structure with urls.py, wsgi.py, 
  and settings.py in a subdirectory under the project's root directory?

* Does this project use a custom layout with a mangled manage.py? 
  For example, do the individual app directories live in the project's root
  directory or are they found in a subdirectory such as "apps"?


Dependencies
~~~~~~~~~~~~
* Is the requirements.txt located in the project's root folder or under
  a subdirectory such as env?

* Does the requirements.txt file itself point to another text file such
  as production.txt located in a subdirectory?

* Are all the requirements pegged to specific version numbers? If not,
  how fast can you run away from this project?


Configuration
~~~~~~~~~~~~~
* Is the settings.py file located in the root directory or under the 
  project name's subdirectory as standard in Django 1.4+?

* Is there a local_settings.py template file?

* Are environment variables required? What's the proper way to set those
  variables?


Data
~~~~
* Which database is the project tested to run with? MySQL, PostgreSQL?

* Are there custom SQL scripts that can only be used with a specific
  database? For example, if the project is designed to run on MySQL are
  there SQL primary keys with autoincrement that would be confusing to
  translate to PostgreSQL?

* Does this project have test data? Is the test data per app, or loaded
  for the entire project?

* How is test data loaded? Data data generation scripts, fixtures, database
  dump SQL scripts, or a database replication scheme?

* If by fixtures or unsure, use "grep -r fixtures.json \*.py" and
  "find . -name '\*.json'" to find one or more JSON fixture files.


Tests
~~~~~
* Are there any tests? If so, do the tests.py files contain anything more
  than the boilerplate 1+1=2 example test function?

* Is there a custom test runner? Is django-nose or django-jenkins used to
  run the tests?

* Are tests grouped into a single tests.py file or are they under a tests/
  subdirectory under each app?

* Are there coverage reports generated for the existing code?



Infrastructure
--------------
Infrastructure is the actual physical or virtualized hardware, or 
platform-as-a-service, that the project is deployed to. Production, staging,
testing, and development servers not on an individual developer's machine
are included in this category.

Note that there are certain things I'm leaving out here. I firmly believe
there is little to no value to "enterprise architecture" type diagrams that
are not generated dynamically. The amount of man hours I've witnessed
ivory tower architects waste on drawing UML diagrams makes my blood boil.


Environments
~~~~~~~~~~~~
* Is there a master "environments document" that is autogenerated with
  information on the infrastructure?

* What environment is the project designed to run in? 

* Linux? What distribution and version? What packages need to be installed?

* Is this project meant to run on a platform-as-a-service? Which one? Heroku? 

* Is there a content delivery network necessary for static and media files? 
  How are the files synced when there is a static files change during 
  deployment?

* Are there separate servers for the database and web? Are there separate
  caching servers? (Obviously this question can scale to many other questions
  depending on the infrastructure size.)

* What are the IP addresses of existing servers?

* Are the testing, staging, and production environments in sync? How do you
  know if they go out of sync?


Access Rights
~~~~~~~~~~~~~
* Are the environemnts accessible with a username and password? 

* Is a public encryption key in the authorized_keys file needed to login?
  Who already has access to add a new key or is there a general key?

* Is root access to a box by SSH denied (as it should be)?


Deployment
~~~~~~~~~~
* Generally speaking, how is a deployment done?

* Are Fabric, Capistrano (railsless-deploy), or shell scripts used?

* Is Ansible, Salt Stack, Puppet, or Chef used?

* What are the purposes of various users? For example, if there is a 
  deployment user as well as another general purpose user, which one
  should be used for debugging?


Third-party services
--------------------
Most Django projects combine custom apps with third party services, 
such as Twilio, Stripe, New Relic, and Intercom.io, to create a complete 
product. Which ones are used in the project, do they fail gracefully when 
their APIs are down or inaccessible, and who has admin access rights to 
the services?

* What third party services are used with this project?

* How are the third party services tested locally?

* What are the usernames and passwords for the services?


