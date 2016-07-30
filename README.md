# Command-WebService
Web Service to serve JSON data for the Ten Commandments app (included).  Includes a mobile version of Ten commandments app

## Command
Web Service to serve commandments and their translations for consumption in JSON data format.  For use by the Ten Commandments app.

This web service takes data from a database and exposes it a JSON object.

Going to the default page takes you to a page where you can :
 *  View the JSON string for all commandments
 *  View the JSON string for all translations
 *  View the JSON string for a number of the commandments [from -> to] by number

This web service uses CodeIgniter and its MVC pattern:
 *  Controller - The logic and the glue between the model and the view.  The controller methods define the public API (Endpoints) for this Web Service.
 *  Model - Governs access to the database.  In this web service data is pulled from the database, and not inserted.
 *  View - The views generate the JSON data.
 
Endpoint Naming Convention
--------------------------
This web service uses URI routing to make more human-friendly URIs for accessing each service.

The endpoint naming convention follows the guidelines given in <a href="http://www.soa-probe.com/2012/10/soa-rest-service-naming-guideline.html">REST naming guidelines</a>.

The main conventions used are :
 *	Use nouns not verbs :  So `getCommandments/` is routed as `commandments/`
 *	Services expecting mandatory arguments over GET should have them as path variables  
       Thus `commandments/2/8` will retrieve commandments from 2 to 8 inclusive
 *	Services offering a uniquely identifiable resource via a key should use basic rest notation  
 		Thus `commandments/5` will retrieve a single commandment (number 5).
 *	All rest service names should use strict low case names.  
 		Hence `commandments` and not `Commandments`.

## Consumer
This is the Web App that uses the JSON data served by the Web Service.

It displays a filterable list of Commandments.  This list is created by calling the getCommandments method of the Web Service, which in turn sends on the Commandments as a JSON object.

Each Commandment in the list is linked with its own page.  Each page contains a list of _translations_.  The translation list is created by calling the getTranslations() method of the Web Service, which sends the translations as a JSON object.

Images for the project are served from an online server.
