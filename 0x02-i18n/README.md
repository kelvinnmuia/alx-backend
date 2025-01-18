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
