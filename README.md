Flasky
======
Another microblogging platform created on basis of the book [Flask Web Development](http://www.flaskbook.com) by Miguel Grinberg.

You are welcome to try it out at http://socialmicroblog.herokuapp.com/ and follow [my profile](http://socialmicroblog.herokuapp.com/user/Norbert)!

##Screenshots
![Profile](http://img5.fotos-hochladen.net/uploads/profilesmz0xn56qe43.jpg)

##Features
- Profile Pages with Avatars
- Follow and Unfollow Users
- Dual language (English/German) support depending on the browser properties.
- Instant translations of blog posts in other languages using Ajax calls.
- OpenID-Login (outdated, more details below)
- Full Text Search (doesn't work on demo site, more details below)

##Planned
- Deployment on Linux VPS

##Requirements
See folder requirements

## Setup instructions (Linux)
###Setup the virtual environment
- `$ virtualenv --version` to check if virtualenv is installed
- `$ sudo apt-get install python-virtualenv`to install viurtalenv
- `$ git clone https://github.com/miguelgrinberg/flasky.git`
- `$ cd flasky`
- Checkout the heroku version: `$ git checkout 17b`
- Overwrite the master branch with the branch you checked out:
	- `$ git merge -s ours master`
	- `$ git checkout master`
	- `$ git merge 17b`
- Create the virtual environment: `$ virtualenv venv`
- Activate the virtual environment: `$ source venv/bin/activate`
- Install all required software packages:`pip install -r requirements.txt`

###Configure the email server
- Please configure your email server by setting the following variables in config.py. If your are using GMail, these are already set to:
	- `MAIL_SERVER = 'smtp.gmail.com'`
	- `MAIL_PORT = 587`
	- `MAIL_USE_TLS = True`
- Set the following local environment variables in the terminal:
	- `$ export MAIL_USERNAME=<mail username>` 
	- `$ export MAIL_PASSWORD=<mail password>`

###Create and fill the database
- Create (or upgrade) database: `$ python manage.py db upgrade`
- Set the administrator email address as local environment variable: `$ export FLASKY_ADMIN=<mail admin>`
- If wanted, generate fake users and posts on your local machine:
	- Install ForgeryPy to generate fake information: $ `pip install forgerypy`
	- `$ python manage.py shell`
	- `>>> User.generate_fake(100)`
	- `>>> Post.generate_fake(100)`

###Run and test the local application
- Start the development server with the application: `$ ./manage.py runserver` or `$ python manage.py runserver` 
**Note:** If you register with the admin mail address, you can edit users, posts and comments and assign roles.)
- To run written unit tests, install the code coverage tool Coverage and the web browser automation tool Selenium: `$ pip install coverage selenium`, then run the tests: `$ ./manage.py test --coverage`
**Note:** A link to a nicer formatted, navigateable HTML report with more detail is displayed at the end of the report.

##Known issues and solutions
- If you want to register a new email-address and get an SMTPAuthenticationError in the Terminal, check your access settings for less secure apps in your mail account. For GMail you find those here: https://www.google.com/settings/security/lesssecureapps. 'Activate' has to be selected for the email function to work.
- If you want to change the default profile image, sign in to https://gravatar.com with the same email address you signed up for flasky and upload a new profile image. This will automatically be shown in your flasky profile afterwards.

##Executables
Run with `$ ./EXECUTABLE.py` from root directory. If permission is denied, give it executable permission with `$ chmod a+x EXECUTABLE.py`.
- run.py: Start up the development web server with the application.
- runp.py: Start up the development web server with the application, but with debugging disabled (production mode).
- tests.py: Run all unittests.
- db_create.py: Create the database.
- db_migrate.py: Generate a database migration after changes to the database structure occured.
- db_upgrade.py: Upgrade the database to the latest revision.
- db_downgrade.py: Downgrade the database one revision. This can be done multiple times to downgrade several revisions.
- tr_init.py: Add a language to the translation catalog.
- tr_compile.py: Update the catalog with new texts from source and templates.
- tr_update.py: Compile the catalog (messages.mo file)

##Deployment (Heroku)
###Creating an account with heroku
- Create an accout with [Heroku](http://heroku.com) and choose the lowest service tier at no cost
- Install the Heroku [Toolbelt](https://toolbelt.heroku.com/) command-line utilities to manage your applications
- Login to Heroku: `$ heroku login`

###Setup the application
- Create a new application on heroku: `$ heroku create <appname>`
- Create a postgresql database: `$ heroku addons:create heroku-postgresql`
- Set heroku configuration environment variable: `$ heroku config:set FLASK_CONFIG=heroku`
- Set the environment variables for the email-server:
	- `$ heroku config:set MAIL_USERNAME=<mail username>` 
	- `$ heroku config:set MAIL_PASSWORD=<mail password>`
- Set the administrator email address as local environment variable: `$ heroku config:set FLASKY_ADMIN=<mail admin>`
**Note:** As the needed requirements.txt file and the Procfile are already included in this repository, they don't need to be created separately anymore.

###Deploying
- Upload the application to the Heroku servers: `$ git push heroku master`
- Execute the deploy command: `$ heroku run python manage.py deploy`
- After the database tables are created and configured, restart the application: `$ heroku restart`


###Run and test the deployed application


If you want to test the Heroku configuration locally,

FLASK_CONFIG=heroku
MAIL_USERNAME=<your-username>
MAIL_PASSWORD=<your-password>

`$ foreman run python manage.py deploy` 


- Install a production-ready web server like Gunicorn inside the virtual environment: `(venv) $ pip install gunicorn` (to start the Gunicorn web server: `(venv) $ gunicorn manage:app`)


