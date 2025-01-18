# 0x02. i18n
## The Domains/Concepts covered in this project: `Back-end`


This project introduced me to internationalization (i18n), a process for adapting applications to support multiple languages and regions. I learned how to use localization frameworks, manage translation files, and implement dynamic content rendering to create a more inclusive and globally accessible backend application.

## Tasks :page_with_curl:

**0. Basic Flask app**

First you will setup a basic Flask app in `0-app.py`. Create a single `/` route and an `index.html` template that simply outputs “Welcome to ALX” as page title (`<title>`) and “Hello world” as header (`<h1>`).

  * [0-app.py](./0-app.py)
  * [templates/0-index.html](./templates/0-index.html)

**1. Basic Babel setup**

Install the Babel Flask extension:

```
$ pip3 install flask_babel==2.0.0
```

Then instantiate the `Babel` object in your app. Store it in a module-level variable named `babel`.

In order to configure available languages in our app, you will create a `Config` class that has a `LANGUAGES` class attribute equal to `["en", "fr"]`.

Use `Config` to set Babel’s default locale (`"en"`) and timezone (`"UTC"`).

Use that class as config for your Flask app.

  * [1-app.py](./1-app.py)
  * [templates/1-index.html](./templates/1-index.html)

**2. Get locale from request**

Create a `get_locale` function with the `babel.localeselector` decorator. Use `request.accept_languages` to determine the best match with our supported languages.

  * [2-app.py](./2-app.py)
  * [templates/2-index.html](./templates/2-index.html)

**3. Parametrize templates**

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

| **msgid**              | **English**              | **French**             |
| :---------             | :----------              | :---------             |
| `home_title`           | `"Welcome to ALX"`       | `"Bienvenue chez ALX"` |
| `home_header`          | `"Hello world"`          | `"Bonjour monde!"`     |

Then compile your dictionaries with

```
$ pybabel compile -d translations
```

Reload the home page of your app and make sure that the correct messages show up.

  * [3-app.py](./3-app.py)
  * [babel.cfg](./babel.cfg)
  * [templates/3-index.html](./templates/3-index.html)
  * [translations/en/LC_MESSAGES/messages.po](./translations/en/LC_MESSAGES/messages.po)
  * [translations/fr/LC_MESSAGES/messages.po](./translations/fr/LC_MESSAGES/messages.po)
  * [translations/en/LC_MESSAGES/messages.mo](./translations/en/LC_MESSAGES/messages.mo)
  * [translations/fr/LC_MESSAGES/messages.mo](./translations/fr/LC_MESSAGES/messages.mo)

**4. Force locale with URL parameter**

In this task, you will implement a way to force a particular locale by passing the `locale=fr` parameter to your app’s URLs.

In your `get_locale` function, detect if the incoming request contains `locale` argument and ifs value is a supported locale, return it. If not or if the parameter is not present, resort to the previous default behavior.

Now you should be able to test different translations by visiting `http://127.0.0.1:5000?locale=[fr|en]`.

**Visiting** `http://127.0.0.1:5000/?locale=fr` **should display this level 1 heading:**

![alt text](./bonjour.png)
