# Project - Catalog Application
## Student: Gordon Seeler / gs6687@att.com
### Date: 1-19-2018 

## Program Files
|Directory|File|Description|
|---------|----|-----------|
|catalog|project.py|main module contains route supt|
|catalog|data_model.py|builds table classes for catalog.db|
|catalog|suptDBFxns.py|fxns for interfacing with SQLAlchemy|
|catalog|suptOAuth2.py|Google Connect and API call fxn|
|catalog|client_secret.json|Client Id and secret key for communicating with Google|
|catalog|catalog.db|SQLite database|
|catalog/static|main.css|style file to control site appearance|
|catalog/templates|main.html|Jinja template main splash page|
|catalog/templates|catitemlist.html|Display all category and items|
|catalog/templates|catitem.html|Display a category item|
|catalog/templates|additem.html|Form to add new category and item|
|catalog/templates|edititem.html|Form to edit a category item|
|catalog/templates|deleteitem.html|Delete a category item|
|catalog/templates|login.html|Jinja temp to login it to sign in with Google|
|catalog/templates|error.html|Page to post application errors|

## Program Summary

The Catalog Application is a website that incorporates Google OAuth2
for Authentication and Flask for Authorization. The site offers an 
inventory of items grouped as categories. New items can be added
for "Logged In" users only. In addition, items may be edited or 
deleted by the owner only. Users of the site who do not login may 
view a static version inventory-- no update.

## Program Description

The Catalog Application is written in Python3. It implements Flask and Jinja
to build a functioning website. 

Data is represented as three different classes: User, Category,
and Item. SQLAlchemy is implemented to establish Object Relational Mapping.
This creates an interface between the underlying database, SQLLite, and
Python replacing native SQL calls with object oriented classes. 

Google+ was selected as the third party OAuth2 service for implementing the Login
feature of the application. 

### Application Database

The **data_model.py** module builds a SQLLite database called **catalog.db**.
Three classes define three tables: User, Category, and Item. 

The User classe contains two important methods: **generate_auth_token** and 
**verfiy_auth_token**. Both implement the creation and validation of a program 
generated user token.

Once a user has been authenticated by Google, a User record is created, and 
a user token is generated for the User's session within the Catalog Application.
The token is built using a random 32 digit number. The encrypted secret key
contains the user's Id and has an expiration of 6 minutes. With it the user
can move about the the application without reauthenticating. The verify_auth_token
will decrypt the user token and either return the user's Id or capture a
bad signature/token expiration condition. 

The Item class provides a **serialize** method used for producing a dictionary 
of an Item record.

### Page and Data Protection

Flask's HTTPBasicAuth is implemented to ensure select routes (ie webpages) are 
protected for only authenticated users. Within the **project.py** module, is the
function **user_verification**. Flask will automatically call this function for any
route defined with login_required attribute. The function is customized to look
for a user token, if found and verified, the Flask **g.user** object will be 
populated with the user's Id. Otherwise, the **g.user** will be set to None. The
functions supporting each route will test the **g.user** object, and if None, it
will set the user's identity to zero. Zero reflects a user who is not logged in
to the application. The functions supporting the routes **will also do a data 
ownership check** making sure that only users who created there inventory can 
update it. 

### OAuth2 Third Party Authentication

The Catalog Application uses Google+ OAuth2 services for authenticating users.
A file **client_secret.json** has been package providing a Client Id and Secret
which enables the application to communicate with Google+'s OAuth2 API. This file
is read in the **project.py** module to obtain Client Id. The Client Id is also 
a hardcoded attribute on Google Sign-In button on the **login.html** page.
The complex back and forth communication between the Catalog Application and 
Google+ will be found in the **suptOAuth2.py** module and the **login.html**
template.

### URI Routes

|URI|Description|
|---------|-----------|
|localhost:8000/|main splash page showing latest items|
|/catalog/_<category>_/items|display a category's items|
|/catalog/_<category>_/_<item>_|display an item's description|
|/catalog/_<item>_/edit|form to edit an item's title or desc|
|/catalog/_<item>_/delete|form to delete an item|
|/catalog/item/new|form to create new catalog and item|
|/catalog/json|return JSON for entire database|
|/catalog/_<category>_/items/json|return JSON for a category's items|
|/catalog/_<category>_/_<item>_/json|return JSON for an item|

### Program Modules

Four Python modules were created for the Catalog Application:
1. data_model.py -- this supports the class table definitions and database.
2. project.py -- this is the main module to invoke, contains support for all the routes
3. suptDBFxns.py -- this is a list of functions which interface with the database.
4. suptOAuth2.py -- this supports the Google connect function.

### Exceptions

All program exceptions caught will be delivered to a error page.

### Messageing

All pages have Flask Messaging. Messages will be presented at the top below the header.

## Prerequisites

1. Port 8000
2. Google Gmail account
3. Python3

## How to run program

1. Create a directory called **catalog** with two subdirectories: **templates** and **static**
2. Copy Python3 files to **catalog**, html files to **templates** and css file **main.css** to **static**
3. Copy the files: **catalog.db** and **client_secret.json** to the main **catalog** directory
4. Issue command $ **python3 program.py**
5. Open browser and issue the URL: http://localhost:8000/
6. Click on categories and/or items 
7. Orange buttons below header will navigate. They also provide login / logoff capability.
8. If logged in, blue buttons with the content reflect update options
9. Login by clicking login button, it will take you to the Google Sign-In button
 











