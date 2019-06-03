<!-- $theme: gaia -->

[Django Tutorial Part 3: Using models](https://github.com/YoonJoon/AboutDjango/blob/master/models.md)
=================================

<br>

##### Created by [이 윤 준](https://www.facebook.com/yoonjoon.lee) (yoonjoon.lee@gmail.com)

May, 2019

---

We'll 

- show how to define models for the LocalLibrary website,
- explain what a model is, how it is declared, and some of the main field types. 
- show briefly a few of the main ways you can access model data.

---

### Overview

Django web applications access and manage data through Python objects referred to as models. 

Models define the structure of stored data, including the field types and possibly also their maximum size, default values, selection list options, help text for documentation, label text for forms, etc. 

The definition of the model is independent of the underlying database.

Once we've chosen what database you want to use, we don't need to talk to it directly at all.

---

### Designing the LocalLibrary models

Let's think about what data we need to store and the relationships between the different objects.

- store information about books (title, summary, author, written language, category, ISBN) and that we might have multiple copies available.
- store more information about the author than just their name, and there might be multiple authors with the same or similar names.
- sort information based on book title, author, written language, and category.

---

When designing our models it makes sense to have separate models for every "object". In this case, the obvious objects are books, book instances, and authors.

We might also want to use models to represent selection-list options. In this case, include the book genre (e.g. Science Fiction, French Poetry, etc.) and language (English, French, Japanese).

Django allows you to define relationships that are one to one (<code>OneToOneField</code>), one to many (<code>ForeignKey</code>) and many to many (<code>ManyToManyField</code>).

---

![](https://mdn.mozillademos.org/files/16479/local_library_model_uml.png)

---

The diagram also shows the relationships between the models, including their multiplicities. 

---

### Model primer

<br>

Explain briefly a overview of how a model is defined and some of the more important fields and field arguments.

---

#### Model definition

Models are usually defined in an application's <b>models.py</b> file. They are implemented as subclasses of <code>django.db.models.Model</code>, and can include fields, methods and metadata. The code fragment shows a "typical" model, named MyModelName:

```python
from django.db import models

class MyModelName(models.Model):
    """A typical class defining a model, derived from the Model class."""

    # Fields
    my_field_name = models.CharField(max_length=20, 
      help_text='Enter field documentation')
    ...
```

---

```python
    # Metadata
    class Meta: 
        ordering = ['-my_field_name']

    # Methods
    def get_absolute_url(self):
        """Returns the url to access a particular 
           instance of MyModelName."""
        return reverse('model-detail-view', 
          args=[str(self.id)])
    
    def __str__(self):
        """String for representing the MyModelName object 
           (in Admin site etc.)."""
        return self.my_field_name
```

---

##### Fields

A model can have an arbitrary number of fields, of any type — each one represents a column of data that we want to store in one of our database tables

Our above example has a single field called <code>my_field_name</code>, of <code>type models.CharField</code>.

The field types are assigned using specific classes, which determine the type of record that is used to store the data in the database, along with validation criteria to be used when values are received from an HTML form.

---

We are giving our field two arguments:

- <code>max_length=20</code> states that the maximum length of a value in this field is 20 characters.
- <code>help_text='Enter field documentation'</code> provides a text label to display to help users know what value to provide when this value is to be entered by a user via an HTML form.

The field name is used to refer to it in queries and templates. Fields also have a label, which is either specified as an argument (verbose_name) or inferred by capitalising the first letter of the field's variable name and replacing any underscores with a space (for example my_field_name would have a default label of My field name).

---

The order that fields are declared will affect their default order if a model is rendered in a form (e.g. in the Admin site), though this may be overridden.

---

###### Common field arguments

The following common arguments can be used:

- help_text
- verbose_name
- default
- null
- blank
- choices
- primary_key

---

###### Common field types

The more commonly used types of fields

- CharField 
- TextField 
- IntegerField 
- DateField and DateTimeField 
- EmailField 
- FileField and ImageField 
- AutoField 
- ForeignKey/ManyToManyField 

---

##### Metadata

You can declare model-level metadata for your Model by declaring <code>class Meta</code>. 

```python
    class Meta:
        ordering = ['-my_field_name']
```

One of the most useful features of this metadata is to control the <i>default ordering</i> of records returned when you query the model type. 

If we chose to sort books like this by default:

```python
	    ordering = ['title', '-pubdate']
```

---

Another common attribute is <code>verbose_name</code>, a verbose name for the class in singular and plural form:

```python
        verbose_name = 'BetterName'
```

Other useful attributes allow
- to create and apply new "access permissions" for the model.
- ordering based on another field, 
- to declare that the class is "abstract"

Many of the other metadata options control what database must be used for the model and how the data is stored.

---

##### Methods

A model can also have methods.

<b>Minimally, in every model you should define the standard Python class method __str__() to return a human-readable string for each object</b>. This string is used to represent individual records in the administration site.

```python
def __str__(self):
    return self.field_name
```

---

Another common method to include in Django models is <code>get_absolute_url()</code>, which returns a URL for displaying individual model records on the website.

```python
def get_absolute_url(self):
    """Returns the url to access a particular 
       instance of the model."""
    return reverse('model-detail-view', 
                   args=[str(self.id)])
```

---

#### Model management

Once you've defined your model classes you can use them to create, update, or delete records, and to run queries to get all records or particular subsets of records.

---

##### Creating and modifying records

To create a record you can define an instance of the model and then call <code>save()</code>.

```python
    # Create a new record using the model's 
    # constructor.
    record = MyModelName(my_field_name="Instance #1")

    # Save the object into the database.
    record.save()
```

---

You can access the fields in this new record using the dot syntax, and change the values. You have to call <code>save()</code> to store modified values to the database.

```python
    # Access model field values using Python attributes.
    print(record.id) # should return 1 for the first record. 
    print(record.my_field_name) # should print 'Instance #1'

    # Change record by modifying the fields, 
    # then calling save().
    record.my_field_name = "New Instance Name"
    record.save()
```

---

##### Searching for records

You can search for records that match certain criteria using the model's objects attribute.

We can get all records for a model as a <code>QuerySet</code>, using <code>objects.all()</code>. The QuerySet is an iterable object, meaning that it contains a number of objects that we can iterate/loop through.

```python
    all_books = Book.objects.all()
```

---

Django's <code>filter()</code> method allows us to filter the returned <code>QuerySet</code> to match a specified text or numeric field against particular criteria. 

```python
    wild_books = Book.objects.filter(title__contains 
                                     = 'wild')
    number_wild_books = wild_books.count()
```

The fields to match and the type of match are defined in the filter parameter name, using the format: <code>field_name__match_type</code>.

---

In some cases you'll need to filter on a field that defines a one-to-many relationship to another model (e.g. a <code>ForeignKey</code>). In this case you can "index" to fields within the related model with additional double underscores.

```python
    # Will match on: Fiction, Science fiction, non-fiction etc.
    books_containing_genre = 
        Book.objects.filter(genre__name__icontains='fiction')
```

---

### Defining the LocalLibrary Models

In this section we will start defining the models for the library. Open <i>models.py (in /locallibrary/catalog/)</i>.

```python
from django.db import models

# Create your models here.
```

---

#### Genre model

This model is used to store information about the book category.

We've created the Genre as a model rather than as free text or a selection list so that the possible values can be managed through the database rather than being hard coded.

---

```python
class Genre(models.Model):
    """Model representing a book genre."""
    name = models.CharField(max_length=200, 
           help_text='Enter a book genre (e.g. Science Fiction)')
    
    def __str__(self):
        """String for representing the Model object."""
        return self.name
```

At the end of the model we declare a <code>\_\_str\_\_()</code> method, which simply returns the name of the genre defined by a particular record.

---

#### Book model

The book model represents all information about an available book in a general sense, but not a particular physical "instance" or "copy" available for loan.

```python
from django.urls import reverse # Used to generate URLs by reversing the URL patterns

class Book(models.Model):
    """Model representing a book 
       (but not a specific copy of a book)."""
    title = models.CharField(max_length=200)

    """Foreign Key used because book can only have 
       one author, but authors can have multiple books.
       Author as a string rather than object 
       because it hasn't been declared yet in the file."""
    author = models.ForeignKey('Author', 
                               on_delete=models.SET_NULL, 
                               null=True)
```

---

```python
    summary = models.TextField(max_length=1000, 
        help_text='Enter a brief description of the book')
    isbn = models.CharField('ISBN', max_length=13, 
        help_text='13 Character <a href="https://www.isbn-international.org/content/what-isbn">ISBN number</a>')
    
    """ManyToManyField used because genre can contain 
       many books. Books can cover many genres.
       Genre class has already been defined so we can 
       specify the object above."""
    genre = models.ManyToManyField(Genre, 
        help_text='Select a genre for this book')
    
    def __str__(self):
        """String for representing the Model object."""
        return self.title
    
    def get_absolute_url(self):
        """Returns the url to access a detail record 
           for this book."""
        return reverse('book-detail', args=[str(self.id)])
```

---

The genre is a <code>ManyToManyField</code>, so that a book can have multiple genres and a genre can have many books. The author is declared as <code>ForeignKey</code>, so each book will only have one author, but an author may have many books.

In both field types the related model class is declared as the first unnamed parameter using either the model class or a string containing the name of the related model. 

You must use the name of the model as a string if the associated class has not yet been defined in this file before it is referenced.

---

The other parameters of interest in the <code>author</code> field are <code>null=True</code>, which allows the database to store a <code>Null</code> value if no author is selected, and <code>on_delete=models.SET_NULL</code>, which will set the value of the author to <code>Null</code> if the associated author record is deleted.

The model also defines <code>\_\_str\_\_()</code>, using the book's <code>title</code> field to represent a <code>Book</code> record. The method, <code>get\_absolute\_url()</code> returns a URL that can be used to access a detail record for this model.

---

#### BookInstance model

The <code>BookInstance</code> represents a specific copy of a book that someone might borrow, and includes information about whether the copy is available or on what date it is expected back, "imprint" or version details, and a unique id for the book in the library.

The model uses

- <code>ForeignKey</code> to identify the associated <code>Book</code>.
- <code>CharField</code> to represent the imprint (specific release) of the book.

---

```python
import uuid # Required for unique book instances

class BookInstance(models.Model):
    """Model representing a specific copy of a book 
       (i.e. that can be borrowed from the library)."""
    id = models.UUIDField(
        primary_key=True, 
        default=uuid.uuid4, 
        help_text='Unique ID for this particular book across whole library')
    book = models.ForeignKey(
        'Book', 
        on_delete=models.SET_NULL, 
        null=True) 
    imprint = models.CharField(max_length=200)
    due_back = models.DateField(null=True, blank=True)
```

---

```python
    LOAN_STATUS = (
        ('m', 'Maintenance'),
        ('o', 'On loan'),
        ('a', 'Available'),
        ('r', 'Reserved'),
    )

    status = models.CharField(
        max_length=1,
        choices=LOAN_STATUS,
        blank=True,
        default='m',
        help_text='Book availability',
    )

    class Meta:
        ordering = ['due_back']

    def __str__(self):
        """String for representing the Model object."""
        return f'{self.id} ({self.book.title})'
```

---

We additionally declare a few new types of field:

- UUIDField 
- DateField 
- status 

The model <code>\_\_str\_\_()</code> represents the <code>BookInstance</code> object using a combination of its unique id and the associated <code>Book</code>'s title.

---

#### Author model

```python
class Author(models.Model):
    """Model representing an author."""
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    date_of_birth = models.DateField(
        null=True, 
        blank=True)
    date_of_death = models.DateField(
        'Died', 
        null=True, 
        blank=True)

    class Meta:
        ordering = ['last_name', 'first_name']
```

---

```python
    def get_absolute_url(self):
        """Returns the url to access a particular author 
           instance."""
        return reverse('author-detail', args=[str(self.id)])

    def __str__(self):
        """String for representing the Model object."""
        return f'{self.last_name}, {self.first_name}'
```

---

### Re-run the database migrations

<br>

Now re-run your database migrations to add them to your database.

```python
py -3 manage.py makemigrations
py -3 manage.py migrate
```

---

### Language model — challenge

Imagine a local benefactor donates a number of new books written in another language. The challenge is to work out how these would be best represented in our library website, and then to add them to the models.

---

Some things to consider:

- Should "language" be associated with a <code>Book</code>, <code>BookInstance</code>, or some other object?
- Should the different languages be represented using model, a free text field, or a hard-coded selection list?

After you've decided, add the field.


