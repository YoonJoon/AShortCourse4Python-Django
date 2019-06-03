<!-- $theme: gaia -->

[Django Tutorial Part 2: Creating a skeleton website](https://github.com/YoonJoon/AboutDjango/blob/master/skeletonWebsite.md)
=================================

<br>

##### Created by [이 윤 준](https://www.facebook.com/yoonjoon.lee) (yoonjoon.lee@gmail.com)

May, 2019

---

### Overview

<br>

It is shown:
- how we can create a "skeleton" website, 
- which we can then populate with site-specific settings, paths, models, views, and templates.

---

The process is straightforward:

<font size="6">

1. Use the <code>django-admin</code> tool to create the project folder, basic file templates, and project management script (<b>manage.py</b>).
2. Use <b>manage.py</b> to create one or more <i>applications</i>.
3. Register the new applications to include them in the project.
4. Hook up the url/path mapper for each application.

</font>

---

For the Local Library website the website folder and its project folder will be named <i>locallibrary</i>, and we'll have just one application named <i>catalog</i>.

<font size="6">

```
locallibrary/         # Website folder
    manage.py         # Script to run Django tools 
                      # for this project (created using django-admin)
    locallibrary/     # Website/project folder (created using django-admin)
    catalog/          # Application folder (created using manage.py)
```
</font>

---

### Creating the catalog application

First open a command prompt/terminal, make sure you are in your virtual environment, navigate to where you want to store your Django apps, and create a folder for your new website (in this case: <i>django_projects</i>). Then enter into the folder using the cd command:

<font size="6">

```bash
mkdir django_projects 
cd django_projects
```
</font>

---

### Creating the project

Create the new project using the <code>django-admin startproject</code> command, and then navigate into the folder.

<font size="6">

```bash
django-admin startproject locallibrary
cd locallibrary
```
</font>

The django-admin tool creates a folder/file structure.

<font size="6">

```bash
locallibrary/
    manage.py
    locallibrary/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```
</font>

---

Our current working directory should look something like this:

<font size="6">

```bash
../django_projects/locallibrary/
```
</font>

The locallibrary project sub-folder is the entry point for the website:

<font size="5">
  
- <b>\_\_init\_\_.py</b> is an empty file that instructs Python to treat this directory as a Python package.
- <b>settings.py</b> contains all the website settings. This is where we register any applications we create, the location of our static files, database configuration details, etc.  
- <b>urls.py</b> defines the site url-to-view mappings. While this could contain all the url mapping code, it is more common to delegate some of the mapping to particular applications, as you'll see later.
- <b>wsgi.py</b> is used to help your Django application communicate with the web server. We can treat this as boilerplate.

</font>

---

The <b>manage.py</b> script is used to create applications, work with databases, and start the development web server. 

---

### Creating the catalog application

Run the following command to create the catalog application.

<font size="6">

```python
py -3 manage.py startapp catalog
```

</font>

The tool creates a new folder and populates it with files for the different parts of the application. Most of the files are usefully named after their purpose (e.g. views should be stored in views.py, models in models.py, tests in tests.py, administration site configuration in admin.py, application registration in apps.py) and contain some minimal boilerplate code for working with the associated objects.

---

The updated project directory should now look like this:

<font size="5">

```
locallibrary/
    manage.py
    locallibrary/
    catalog/
        admin.py
        apps.py
        models.py
        tests.py
        views.py
        __init__.py
        migrations/
```
</font>

<font size="5">

- A <i>migrations</i> folder, used to store "migrations" — files that allow you to automatically update your database as you modify your models. 
- <b>\_\_init\_\_.py</b>  — an empty file created here so that Django/Python will recognise the folder as a Python Package and allow you to use its objects within other parts of the project.

</font>

---

### Registering the catalog application

The application <i>catalog</i> has been created we have to register it with the project so that it will be included when any tools are run. Applications are registered by adding them to the <code>INSTALLED_APPS</code> list in the project settings. 

---

Open the project settings file <b>django_projects/locallibrary/locallibrary/settings.py</b> and find the definition for the <code>INSTALLED_APPS</code> list. Then add a new line at the end of the list, as shown in bold below.

<font size="6">

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'catalog.apps.CatalogConfig', 
]
```

</font>

The new line specifies the application configuration object (<code>CatalogConfig</code>). 

---

### Specifying the database

The point where you would normally specify the database to be used for the project.

We'll use the SQLite database for this example and show how this database is configured in <b>settings.py</b>.

<font size="6">

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

</font>

---

### Other project settings

The <b>settings.py</b> file is also used for configuring a number of other settings. 

We probably only want to change the <code>TIME\_ZONE</code>.

```python
TIME_ZONE = 'Asia/Seoul'
```

---

We should be aware of:

- <code>SECRET_KEY</code>. This is a secret key that is used as part of Django's website security strategy. If you're not protecting this code in development, you'll need to use a different code (perhaps read from an environment variable or file) when putting it into production.
- <code>DEBUG</code>. This enables debugging logs to be displayed on error, rather than HTTP status code responses. This should be set to <code>False</code> on production as debug information is useful for attackers, but for now we can keep it set to <code>True</code>.

---

### Hooking up the URL mapper

The website is created with a URL mapper file (<code>urls.py</code>) in the project folder. It is more usual to defer mappings to the associated application.

---

<font size="5">
  
```python
"""locallibrary URL Configuration

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/2.1/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```
</font>

---

Add a new list item to the urlpatterns list.

<font size="5">
  
```python
# Use include() to add paths from the catalog application 
from django.urls import include

urlpatterns += [
    path('catalog/', include('catalog.urls')),
]
```
</font>

Now let's redirect the root URL of our site (i.e. <code>127.0.0.1:8000</code>) to the URL <code>127.0.0.1:8000/catalog/</code>. Add again to the bottom of the file:

<font size="5">
  
```python
#Add URL maps to redirect the base URL to our application
from django.views.generic import RedirectView

urlpatterns += [
    path('', RedirectView.as_view(url='/catalog/', 
         permanent=True)),
]
```
</font>

---

Django does not serve static files like CSS, JavaScript, and images by default, but it can be useful for the development web server to do so while you're creating your site. 

<font size="5">
  
```python
# Use static() to add url mapping to serve static files during development (only)
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.STATIC_URL, 
                      document_root=settings.STATIC_ROOT)
```
</font>

----

As a final step, create a file inside your catalog folder called urls.py, and add the following text to define the (empty) imported urlpatterns.

<font size="5">
  
```python
from django.urls import path
from . import views

urlpatterns = [

]
```
</font>

---

### Testing the website framework

<br>

At this point we have a complete skeleton project. The website doesn't actually do anything yet, but it's worth running it to make sure that none of our changes have broken anything. 

---

#### Running database migrations

As we change our model definitions, Django tracks the changes and can create database migration scripts (in <b>/locallibrary/catalog/migrations/</b>) to automatically migrate the underlying data structure in the database to match the model.

```bash
py -3 manage.py makemigrations
py -3 manage.py migrate
```

The <code>makemigrations</code> command creates the migrations for all applications installed in your project. The <code>migrate</code> command actually applies the migrations to your database.

---

#### Running the website

Run the development web server by calling the runserver command in the same directory as <b>manage.py</b>:

```python
python3 manage.py runserver

 Performing system checks...

 System check identified no issues (0 silenced).
 August 15, 2018 - 16:11:26
 Django version 2.1, using settings 'locallibrary.settings'
 Starting development server at http://127.0.0.1:8000/
 Quit the server with CTRL-BREAK.
```

---

Once the server is running you can view the site by navigating to http://127.0.0.1:8000/ in your local web browser.

![](https://mdn.mozillademos.org/files/15729/django_404_debug_page.png)

---

### Challenge yourself

<br>

A URL-mapping for the Admin site has already been added in the project's <b>urls.py</b>. Navigate to the admin area in your browser and see what happens.




