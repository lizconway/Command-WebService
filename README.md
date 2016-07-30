# WebService
Web Service to serve commandments and their translations for consumption in JSON data format.  For use by the Ten Commandments app.

This web service takes data from a database and exposes it a JSON object.

Going to the default page takes you to a page where you can :
 *  View the JSON string for all commandments
 *  View the JSON string for a single commandment
 *  View the JSON string for subsets of the commandments
 *  View the JSON string for all translations

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

Base URL
--------
On my local PC this project is in _C:\xampp\htdocs\ServerSide\Assessment\WebService_.  
In .htaccess the RewriteBase command is written as `RewriteBase /ServerSide/Assessment/WebService/`  
In application/config/config.php the base_url is set as `$config['base_url'] = 'http://localhost/ServerSide/Assessment/WebService/';

URI Routing
-----------
CodeIgniter's URI routing is used to make a more user-friendly URI when accessing the commandments and data.
 * Instead of using `index.php/CommandCtrl/getCommandments/0/0` you can use `index.php/commandments` to retrieve all of the commandments.
 * Instead of using `index.php/CommandCtrl/getTranlations` you can use `index.php/translations` to retrieve all of the tranlations.
 * You can also retrieve a subset of the commandments by using `index.php/commandments/1/10`, to retrieve commandments 1 to 10 (use any numbers between 1 and 11).  If a higher number is put first the commandments will be retrieved in reverse order.  E.g. `index.php/commandments/8/3` will retrieve the commandments starting with 8 and working backwards to 3.
 * You can also retrieve a single commandment with the URI `index.php/commandment/5`, to retrieve only commandment 5 for example.

Customer Error Handler
----------------------
There is a custom routing used to override the Error handler for '404 Page not found' errors.  
The custom error handler is `Error404.php`.
 
## Consumer
This is the Web App that uses the JSON data served by the Web Service.

It displays a filterable list of Commandments.  This list is created by calling the `index.php/commandments` endpoint of the Web Service, which in turn sends on the Commandments as a JSON object.  This consumer always requests all the Commandments, not any subset of Commandments.

Each Commandment in the list is linked with its own page.  Each page contains a list of _translations_.  The translation list is created by calling the `index.php/translations` endpoint of the Web Service, which sends the translations as a JSON object.

Images for the project are served from an online server.
