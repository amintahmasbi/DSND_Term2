# Flask

## 1. Installing Flask

0. (Optional) create a virtual environment

1. ```shell
   (venv) $ pip install flask
   ```

## 2. project structure

```
myproject/
  venv/ (DO NOT DEPLOY)
  myapp/
    __init__.py
    routes.py
  myproject.py
  .flaskenv
```

The application `myapp` will exist in a *package*. In Python, a sub-directory that includes a `__init__.py` file is considered a package, and can be imported. When you import a package, the `__init__.py` executes and defines what symbols the package exposes to the outside world.

## 3. A "Hello, World" Flask Application

`myapp/__init__.py`:

```python
from flask import Flask

app = Flask(__name__)

from myapp import routes
```

The `routes` module is imported at the bottom and not at the top of the script as it is always done. The bottom import is a workaround to *circular imports*, a common problem with Flask applications. You are going to see that the `routes` module needs to import the `myapp` variable defined in this script, so putting one of the reciprocal imports at the bottom avoids the error that results from the mutual references between these two files.

`myapp/routes.py`:

```python
from myapp import app

@app.route('/')
@app.route('/index')
def index():
    return "Hello, World!"
```

There are two decorators, which associate the URLs `/` and `/index` to this function. This means that when a web browser requests either of these two URLs, Flask is going to invoke this function and pass the return value of it back to the browser as a response.

`myproject.py`:

```python
from myapp import app
```

This first version of the application is now complete! 

(method 1) Before running it, though, Flask needs to be told how to import it, by setting the `FLASK_APP` environment variable:

```shell
(venv) $ export FLASK_APP=myproject.py
```

(method 2) Since environment variables aren't remembered across terminal sessions, you want to automatically import them when you run the `flask` command:

```shell
(venv) $ pip install python-dotenv
```

Then you can just write the environment variable name and value in a `.flaskenv` file in the top-level 		directory of the project (Also `.env`  for Heroku deployment):

`.flaskenv`:

```
FLASK_APP=myproject.py
```

You can now run your first web application, with the following command:

```shell
(venv) $ flask run
 * Serving Flask app "microblog"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

## 4. Templates (to store HTML files)

Add a templates folder to the package:

```shell
(venv) $ mkdir myapp/templates
```

Write this file in `myapp/templates/index.html`:

```html
<html>
    <head>
        <title>{{ title }} - Myproject</title>
    </head>
    <body>
        <h1>Hello, {{ user.username }}!</h1>
    </body>
</html>
```

Placeholders for the dynamic content, enclosed in `{{ ... }}` sections. These placeholders represent the parts of the page that are variable and will only be known at runtime.

Update `myapp/routes.py`:

```python
from flask import render_template
from myapp import app

@app.route('/')
@app.route('/index')
def index():
    user = {'username': 'Miguel'}
    return render_template('index.html', title='Home', user=user)
```

The `render_template()` function invokes the [Jinja2](http://jinja.pocoo.org/) template engine that comes bundled with the Flask framework. Jinja2 substitutes `{{ ... }}` blocks with the corresponding values, given by the arguments provided in the `render_template()` call.

Jinja2 supports control statements, given inside `{% ... %}` blocks.

### Template Inheritance

Jinja2 has a template inheritance feature that specifically addresses the "repeat yourself" problem.

Define a base template called `base.html` that includes a simple navigation bar and also the title logic.

`app/templates/base.html`:

```html
<html>
    <head>
      {% if title %}
      <title>{{ title }} - Myproject</title>
      {% else %}
      <title>Welcome to Myproject</title>
      {% endif %}
    </head>
    <body>
        <div>Myproject: <a href="/index">Home</a></div>
        <hr>
        {% block content %}{% endblock %}
    </body>
</html>
```

In this template I used the `block` control statement to define the place where the derived templates can insert themselves.

`app/templates/index.html`: (Inherit from base template)

```html
{% extends "base.html" %}

{% block content %}
    <h1>Hi, {{ user.username }}!</h1>
    {% for post in posts %}
    <div><p>{{ post.author.username }} says: <b>{{ post.body }}</b></p></div>
    {% endfor %}
{% endblock %}
```

## 5. Flask + Plotly

a Plotly visualization is set up on the back-end inside the `routes.py` file using Plotly's Python library. The Python plotly code is a dictionary of dictionaries. The Python dictionary is then converted to a JSON format and sent to the front-end via the `render_templates()` method.

On the front-end, the ids and visualization code (JSON code) is then used with the Plotly javascript library to render the plots.

`myapp/routes.py`:

```python
from myapp import app
import json, plotly
from flask import render_template
from wrangling_scripts.wrangle_data import return_figures # get plotly figures

@app.route('/')
@app.route('/index')
def index():

    figures = return_figures()

    # plot ids for the html id tag
    ids = ['figure-{}'.format(i) for i, _ in enumerate(figures)]

    # Convert the plotly figures to JSON for javascript in html template
    figuresJSON = json.dumps(figures, cls=plotly.utils.PlotlyJSONEncoder)

    return render_template('index.html',
                           ids=ids,
                           figuresJSON=figuresJSON)
```

`myapp/templates/index.html`:

```html
...
<head>
    <!--import script files needed from plotly and bootstrap-->
</head>
...
			<!--top two charts-->       
            <div class="row">
                <div class="col-6">
                    <div id="{{ids[0]}}"></div>
                </div>
                <div class="col-6">
                    <div id="{{ids[1]}}"></div>
                </div>
            </div>
...
<footer>
    <script type="text/javascript">
        // plots the figure with id
        // id much match the div id above in the html
        var figures = {{figuresJSON | safe}};
        var ids = {{ids | safe}};
        for(var i in figures) {
            Plotly.plot(ids[i],
                figures[i].data,
                figures[i].layout || {});
        }
    </script>
</footer>
```

## 6. Simple Heroku Deployment

1. pip install the Python libraries needed for the web app:

   ```shell
   (venv) $ pip install flask pandas plotly gunicorn
   ```

2. create a requirements file, which lists all of the Python library that your app depends on:

   ```shell
   (venv) $ pip freeze > requirements.txt
   ```

3. create a `Procfile`, which tells Heroku what to do when starting your web app: 

   `Procfile`:

   ```
   web gunicorn myapp:app
   ```

4. Make your project a git repository (if not already):

   ```shell
   (venv) $ git init
   (venv) $ git add .
   (venv) $ git commit -m "first commit"
   ```

5. Install the heroku command line tools and login:

   ```shell
   (venv) $ sudo snap install --classic heroku
   (venv) $ heroku login
   ```

6. create a heroku app: (name is optional)

   ```shell
   (venv) $ heroku create (my-unique-app-name)
   ```

   The `heroku create` command should create a git repository on Heroku and a web address for accessing your web app. A remote repository named `heroku` is added to your git repository.

7. Finally, deploy by pushing your git repository to the remote heroku repository:

   ```shell
   (venv) $ git push heroku master
   ```

## 7. Flask Web Forms

```shell
(venv) $ pip install flask-wtf
```

Use a class to store configuration variables. To keep things nicely organized, create the configuration class in a separate Python module.

`config.py`:

```python
import os

class Config(object):
    SECRET_KEY = os.environ.get('SECRET_KEY') or 'you-will-never-guess'
```

Now that I have a config file, I need to tell Flask to read it and apply it.

`myapp/__init__.py`:

```python
from flask import Flask
from config import Config

app = Flask(__name__)
app.config.from_object(Config)

from myapp import routes
```

use a new `myapp/forms.py` module to store my web form classes:

`myapp/forms.py`:

```python
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField, BooleanField, SubmitField
from wtforms.validators import DataRequired

class LoginForm(FlaskForm):
    username = StringField('Username', validators=[DataRequired()])
    password = PasswordField('Password', validators=[DataRequired()])
    remember_me = BooleanField('Remember Me')
    submit = SubmitField('Sign In')
```

`myapp/routes.py`:

```python
from flask import render_template, flash, redirect

@app.route('/login', methods=['GET', 'POST'])
def login():
    form = LoginForm()
    if form.validate_on_submit():
        flash('Login requested for user {}, remember_me={}'.format(
            form.username.data, form.remember_me.data))
        return redirect('/index')
    return render_template('login.html', title='Sign In', form=form)
```

[read more...](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-iii-web-forms)

## i. Database

