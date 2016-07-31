# WebService
A CodeIgniter generated Web Service to serve commandments and their translations for consumption in JSON data format.  For use by the Ten Commandments app.

This web service takes data from a database and exposes it a JSON object.

Going to the default page takes you to a page where you can :
 *  View the JSON string for all commandments
 *  View the JSON string for a single commandment
 *  View the JSON string for subsets of the commandments
 *  View the JSON string for all translations
 *  View the JSON string for a single translation

This web service uses CodeIgniter and its MVC pattern:
 *  Controller - The logic and the glue between the model and the view.  The controller methods define the public API (Endpoints) for this Web Service.
 *  Model - Governs access to the database.  In this web service data is only pulled from the database, and not inserted.  (HTTP GET verb only).
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
 * The 'index.php' that comes before the Controller name has been hidden using a mod_rewrite rule in .htaccess
 * Instead of using `index.php/CommandCtrl/getCommandments/0/0` you can use `commandments` to retrieve all of the commandments.
 * Instead of using `index.php/CommandCtrl/getTranlations` you can use `translations` to retrieve all of the tranlations.
 * You can also retrieve a subset of the commandments by using `commandments/1/10`, to retrieve commandments 1 to 10 (use any numbers between 1 and 11).  If a higher number is put first the commandments will be retrieved in reverse order.  E.g. `commandments/8/3` will retrieve the commandments starting with 8 and working backwards to 3.
 * You can also retrieve a single commandment with the URI `commandment/5`, to retrieve only commandment 5 for example.
 * You can retrieve a particular translation by supplying the txt you want translated as a parameter.  E.g. `translations/srsly`.  
Only one translation can be retrieved in this manner.  If multiple parameters are used the WebService will combine them and search for a translation for the combined parameters - including a slash between them.  E.g `translations/w/end` will search for a translation for the string "_w/end_".
This WebService allows searching for translations for text with spaces, e.g. `translations/4 now`.  
Certain characters are disallowed in the URI and so text containing these characters cannot be searched for even if they exist in the database.  `permitted_uri_chars` configuration was updated in the config.php to allow words like "_m&d_" or "_Oh hai!_" to be searched for.

Errors
------
There is a custom routing used to override the Error handler for '404 Page not found' errors.  
The custom error handler is `Error404.php`.  
If you enter a parameter that does not exist an empty JSON object is returned.  E.g. `commandments/13`, or `translations/whatever` will return `{"commandments":[]}` and `{"textSpeak":[]}` respectively.  
If you enter a negative parameter for the commandments a 404 error is thrown.
 
## Consumer
This is the Web App that uses the JSON data served by the Web Service.

It displays a filterable list of Commandments.  This list is created by calling the `commandments` endpoint of the Web Service, which in turn sends on the Commandments as a JSON object.  This consumer always requests all the Commandments, not any subset of Commandments.

Each Commandment in the list is linked with its own page.  Each page contains a list of _translations_.  The translation list is created by calling the `translations` endpoint of the Web Service, which sends the translations as a JSON object.

Images for the project are served from an online server.
