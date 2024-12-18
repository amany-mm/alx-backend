# 0x02. i18n
#### `Back-end`
![91e1c50322b2428428f9](https://s3.amazonaws.com/alx-intranet.hbtn.io/uploads/medias/2020/1/91e1c50322b2428428f9.jpeg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIARDDGGGOUSBVO6H7D%2F20241106%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20241106T002423Z&X-Amz-Expires=86400&X-Amz-SignedHeaders=host&X-Amz-Signature=4737994e5fadb4b1db85e799b7480b76dfbfd5b827e80fa1147d41f63b7cc475)

## Resources
**Read or watch**:

* [Flask-Babel](https://python-babel.github.io/flask-babel/)
* [Flask i18n tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-xiii-i18n-and-l10n)
* [pytz](https://pytz.sourceforge.net/)

## Requirements
* All your files will be interpreted/compiled on Ubuntu `18.04` LTS using `python3` (version `3.7`)
* All your files should end with a new line
* A `README.md` file, at the root of the folder of the project, is mandatory
* Your code should use the `pycodestyle` style (version `2.5`)
* The first line of all your files should be exactly `#!/usr/bin/env python3`
* All your `*.py` files should be executable
* All your modules should have a documentation (`python3 -c 'print(__import__("my_module").__doc__)'`)
* All your classes should have a documentation (`python3 -c 'print(__import__("my_module").MyClass.__doc__)'`)
* All your functions and methods should have a documentation (`python3 -c 'print(__import__("my_module").my_function.__doc__)'` and `python3 -c 'print(__import__("my_module").MyClass.my_function.__doc__)'`)
* A documentation is not a simple word, it’s a real sentence explaining what’s the purpose of the module, class or method (the length of it will be verified)
* All your functions and coroutines must be type-annotated.

## Tasks

[0. Basic Flask app](./0-app.py)

First you will setup a basic Flask app in `0-app.py`. Create a single `/` route and an `index.html` template that simply outputs “Welcome to Holberton” as page title (`<title>`) and “Hello world” as header (`<h1>`).


[1. Basic Babel setup](./1-app.py)

Install the Babel Flask extension:
```
$ pip3 install flask_babel==2.0.0
```
Then instantiate the `Babel` object in your app. Store it in a module-level variable named `babel`.

In order to configure available languages in our app, you will create a `Config` class that has a LANGUAGES class attribute equal to `["en", "fr"]`.

Use `Config` to set Babel’s default locale (`"en"`) and timezone (`"UTC"`).

Use that class as config for your Flask app.


[2. Get locale from request](./2-app.py)

Create a `get_locale` function with the `babel.localeselector` decorator. Use `request.accept_language`s to determine the best match with our supported languages.


[3. Parametrize templates](./3-app.py)

Use the `_` or `gettext` function to parametrize your templates. Use the message IDs `home_title` and `home_header`.

Create a `babel.cfg` file containing
```
[python: **.py]
[jinja2: **/templates/**.html]
extensions=jinja2.ext.autoescape,jinja2.ext.with_
```
Then initialize your translations with
```
$ pybabel extract -F babel.cfg -o messages.pot .
```
and your two dictionaries with
```
$ pybabel init -i messages.pot -d translations -l en
$ pybabel init -i messages.pot -d translations -l fr
```
Then edit files `translations/[en|fr]/LC_MESSAGES/messages.po` to provide the correct value for each message ID for each language. Use the following translations:

| msgid | English | French |
| --- | --- | --- |
| `home_title` | `"Welcome to Holberton"` | `"Bienvenue chez Holberton"` |
| `home_header` | `"Hello world!"` | `"Bonjour monde!"` |

Then compile your dictionaries with
```
$ pybabel compile -d translations
```
Reload the home page of your app and make sure that the correct messages show up.


[4. Force locale with URL parameter](./4-app.py)

In this task, you will implement a way to force a particular locale by passing the `locale=fr` parameter to your app’s URLs.

In your `get_locale` function, detect if the incoming request contains `locale` argument and ifs value is a supported locale, return it. If not or if the parameter is not present, resort to the previous default behavior.

Now you should be able to test different translations by visiting `http://127.0.0.1:5000?locale=[fr|en]`.

##### Visiting `http://127.0.0.1:5000/?locale=fr` should display this level 1 heading:
<img width="254" alt="f958f4a1529b535027ce" src="https://s3.amazonaws.com/alx-intranet.hbtn.io/uploads/medias/2020/3/f958f4a1529b535027ce.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIARDDGGGOUSBVO6H7D%2F20241106%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20241106T002423Z&X-Amz-Expires=86400&X-Amz-SignedHeaders=host&X-Amz-Signature=78dcc1c1d9e3e4466d0d30b46724592f7c32a321a0fcd0417e4c45eb80390458">


[5. Mock logging in](./5-app.py)

Creating a user login system is outside the scope of this project. To emulate a similar behavior, copy the following user table in `5-app.py`.
```
users = {
    1: {"name": "Balou", "locale": "fr", "timezone": "Europe/Paris"},
    2: {"name": "Beyonce", "locale": "en", "timezone": "US/Central"},
    3: {"name": "Spock", "locale": "kg", "timezone": "Vulcan"},
    4: {"name": "Teletubby", "locale": None, "timezone": "Europe/London"},
}
```
This will mock a database user table. Logging in will be mocked by passing `login_as` URL parameter containing the user ID to log in as.

Define a `get_user` function that returns a user dictionary or `None` if the ID cannot be found or if `login_as` was not passed.

Define a `before_request` function and use the `app.before_request` decorator to make it be executed before all other functions. `before_request` should use `get_user` to find a `user` if any, and set it as a global on `flask.g.user`.

In your HTML template, if a user is logged in, in a paragraph tag, display a welcome message otherwise display a default message as shown in the table below.

| msgid | English | French |
| --- | --- | --- |
| `logged_in_as` | `"You are logged in as %(username)s."` | `"Vous êtes connecté en tant que %(username)s."` |
| `not_logged_in` | `"You are not logged in."` | `"Vous n'êtes pas connecté."` |

##### Visiting `http://127.0.0.1:5000/` in your browser should display this:
<img width="213" alt="2c5b2c8190f88c6b4668" src="https://s3.amazonaws.com/alx-intranet.hbtn.io/uploads/medias/2020/3/2c5b2c8190f88c6b4668.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIARDDGGGOUSBVO6H7D%2F20241106%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20241106T002423Z&X-Amz-Expires=86400&X-Amz-SignedHeaders=host&X-Amz-Signature=4e8ce42697c3a61729eb15d23bb20f5e944cd33dc8bb07ba2eb285874abf71c7">

##### Visiting `http://127.0.0.1:5000/?login_as=2` in your browser should display this:
<img width="259" alt="277f24308c856a09908c" src="https://s3.amazonaws.com/alx-intranet.hbtn.io/uploads/medias/2020/3/277f24308c856a09908c.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIARDDGGGOUSBVO6H7D%2F20241106%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20241106T002423Z&X-Amz-Expires=86400&X-Amz-SignedHeaders=host&X-Amz-Signature=65edbf0cd8afa5d669995521e756062136f5be0290cb600b66e8a81c8293514b">


[6. Use user locale](./6-app.py)

Change your `get_locale` function to use a user’s preferred local if it is supported.

The order of priority should be

1. Locale from URL parameters
2. Locale from user settings
3. Locale from request header
4. Default locale

Test by logging in as different users

<img width="272" alt="9941b480b0b9d87dc5de" src="https://s3.amazonaws.com/alx-intranet.hbtn.io/uploads/medias/2020/3/9941b480b0b9d87dc5de.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIARDDGGGOUSBVO6H7D%2F20241106%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20241106T002423Z&X-Amz-Expires=86400&X-Amz-SignedHeaders=host&X-Amz-Signature=dc66fed055c6dda87def09d73feedbc9678e2c0b0e49d71fb49d1ba9649c3dc1">

