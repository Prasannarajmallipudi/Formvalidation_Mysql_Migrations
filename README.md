# Form Validation

## Forms
Forms are a collection of HTML elements to take input from the user. In simple words, whenever you give any kind of instruction or input on web, that is known as a form.
The input elements come inside ```<form></form>``` tags. It is collectively called as HTML form. 
Some daily examples of forms can include:
* Google search
* Facebook posts and stories
* Online registrations

There are different methods to transmit data over HTTP. The 2 very popular methods among all of them are GET and POST.
____

### HTML Forms

The ``` <form> ``` tag has two important attributes to be specified:

#### 1. Action
This attribute specifies where to submit the form data.It can be left blank but in that case, the data will be sent to the same URL. Otherwise, you can specify a URL. This attribute just tells the form where to submit the data.
**Example:

```html
<form action="{% url 'Reg' %}"> 
</form>
```
#### 2. Method
This attribute specifies how to submit data. There are GET and POST values for the same. You should use capital values for this attribute.
**Example:

```html
<form action="" method="POST/GET>
</form>
```
____

#### Django Forms Validation

Django provides built-in methods to validate form data automatically. Django forms submit only if it contains CSRF tokens.
the Django forms
* Forms
* HTTP, GET & POST Method
* Django Forms
* Django Form Class
* Django Form Validation

![GitHub Logo](/images/Djangoforms.jpg)

Form validation means verification of form data. Form validation is an important process when we are using model forms. When we validate our form data, we are actually converting them to python datatypes. Then we can store them in the database.
The is_valid() method is used to perform validation for each field of the form, it is defined in Django Form class. It returns True if data is valid and place all data into a cleaned_data attribute.

##### Django Validation Example

* First create a new Project(Project_Name)
     ``` django-admin startproject Project_Name ```
  * and after create a app (Newly Created Application)
    ``` python manage.py startapp firstapp ```
    
First open settings.py File and add INSTALLED_APPS( Newly Created Application_firstapp)
Edit your settings.py
```python
INSTALLED_APPS = [
    'firstapp',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

Created a follow the model fields and implement the Model given fileds 

// models.py

 edit your modles.py

```python
from django.db import models
# Create your models here.
class Register(models.Model):
	vals = [('Male','Male'),('Female','Female')]
	firstName = models.CharField(max_length=10)
	lastName = models.CharField(max_length=10)
	emailId = models.EmailField(null=True)
	phoneNo = models.CharField(max_length=10)
	age = models.IntegerField(null=True)
	gender = models.CharField(max_length=10,choices=vals)
	date_of_birth = models.DateField(null=True)
	

	def __str__(self):
		return self.emailId
```

Now, create a form in your application (create a new file in your app ``forms.py`` File)

 edit your forms.py

// forms.py

```python
from django.forms import ModelForm
from firstapp.models import Register

class RegisterForm(ModelForm):
	class Meta:
		model = Register
		fields = "__all__"
```

The form, check whether request is post or not. It validate the data by using is_valid() method.

edit your views.py

//views.py
```python
from django.shortcuts import render, redirect
from django.http import HttpResponse
from firstapp.forms import RegisterForm

# Create your views here.
def Reg(request):
	if request.method=='POST':
		form=RegisterForm(request.POST)
		if form.is_valid():
			form.save()
			return HttpResponse('Registered Successfully ')

	form=RegisterForm()
	return render(request, 'firstapp/index.html',{'form':form})
```

The ```urls.py``` File can be add the ```path and import the path,views```

 edit your urls.py
 
// urls.py

```python
from django.contrib import admin
from django.urls import path
from firstapp import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('Reg/', views.Reg, name='Reg'),
    path('Show/', views.Show, name='Show'),
]
```

Now create a folder in ```templates``` Folder and create one filename.html file and write code in that file 
Index template that shows form and errors.

// index.html
```html
<!DOCTYPE html>
{% load static %}
<html>
<head>
	<title>Index Page</title>
</head>
<body>
	<form action="{% url 'Reg' %}" method="POST">
		{% csrf_token %}
		{{ form.as_p }}
		<input type="submit" value="Register">
  </form>
</body>
</html>
```

Note : Run the server before run these commands in project location into Command prompt
     ```python manage.py makemigrations```
     and
     ```python manage.py migtare```

### Run the Server

```python manage.py runserver 2422```

Open the browser and Type : localhost:2422/Reg

![GitHub Logo](/images/Reg.png)


## How To Create a Django App and Connect it to a Database
* First the check XAMPP Server installed or not in your local machine.
* If not installed install now
	*Software Download link is : [GitHub](https://www.apachefriends.org/download.html)

* After installation completed Start the ``Apache`` and ``MySQL``
* Goto MySQL Admin Button click to open the ```phpmyadmin``` Page
Web Application using ``django-admin``, creating the MySQL database and then connecting the web application to the database.

Open the project and Goto setting.py File 
edit your settings.py
* Add ENGINE
* NAME
* USER
* PASSWORD
* PORT
* HOST
 
// settings.py
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'task_reg',
        'USER': 'root',
        'PASSWORD':'',
        'PORT':'',
        'HOST':'',
    }
}
```
Save the file.

Open command prompt in project location run the commands is:
```python manage.py makemigrations```
     and
```python manage.py migtare```
and Running the server 
``` python manage.py runserver 2422 ```

After open the ```phpmyadmin``` Page and Table is created and after Registerd form fill the data and click the register button data will be stored in Data Base Table.

![GitHub Logo](/images/mysqlpage.png)








____






