# Formvalidation_Mysql_Migrations

## Form Validations

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

```python
<form action="{% url 'Reg' %}"> 
</form>
```
#### 2. Method
This attribute specifies how to submit data. There are GET and POST values for the same. You should use capital values for this attribute.
**Example:

```python
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

![Github Image](Django-forms.JPG)

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

Note : After completion of email sending turn off less secure apps option




















        
![Github Image](secure.JPG)

Note : After completion of email sending turn off less secure apps option
____
### Add smtp and gmail account details in settings.py file existed in project folder
```python
EMAIL_USE_TLS = True  
EMAIL_HOST = 'smtp.gmail.com'  
EMAIL_PORT = 587  
EMAIL_HOST_USER = 'sender_username@gmail.com'
EMAIL_HOST_PASSWORD = '********'
```
____
### Create forms.py file in app folder for generating a form with subject, email, message, file fields by using below lines of code
```python
from django import forms  
class EmailForm(forms.Form):      
    email     = forms.EmailField(label = '', max_length = 40, 
    	        widget=forms.EmailInput(attrs={'placeholder':'Enter Sender Email'}))
    subject  = forms.CharField(label = '', max_length = 60, 
    	       widget=forms.TextInput(attrs={'placeholder':'Enter Subject of Email'}))
    body  = forms.CharField(label = '', max_length = 100, 
    	    widget = forms.Textarea(attrs = {'placeholder':'Enter Email Body'}))  
    file      = forms.FileField(label = '') # for creating file input
 
```
____
### In views.py from app folder create a view for sending an email as shown below

```python
from django.shortcuts import render
from django.http import HttpResponse
from email_sending.forms import EmailForm
from myProject import settings
from django.core.mail import EmailMessage

# Create your views here.

def sendMail(request):
	if request.method == 'POST':
		to_mail = request.POST['email']
		from_mail = settings.EMAIL_HOST_USER
		email_sub = request.POST['subject']
		email_body = request.POST['body']
		file_name = request.POST['file']
		mail = EmailMessage(email_sub, email_body, from_mail, [to_mail])
		mail.attach_file(settings.STATIC_ROOT+settings.MEDIA_URL+file_name)
		mail.send()
		return HttpResponse("<h3 style = 'color:green'>Email sent successfully..!!!</h3>")
	email_form = EmailForm()
	return render(request, 'email.html', {'form' : email_form})
```

* In this case myProject is projectname and email_sending is appname
	* Imported settings from myProject to access host email address and static file path
	* Imported EmailForm from forms.py in email_sending app
* EmailMessage is the classname in django.core.mail used for sending an email this class needs subject of email, body of email, sender mail, receiver mail parameters
	* attch_file() is function of EmailMessage class to send an attacment along with email, It requires file path
____
### Create email.html file in templates folder existed in app folder with below lines of html code
```html
<!DOCTYPE html>
<html>
<head>
	<title>Send Mail</title>
</head>
<body>
<h3>Email Sending</h3>
<form action="{% url 'send_mail' %}" method="POST">
	{% csrf_token %}
	{{ form.as_p }}
	<button type="submit">Send</button>  
</form>
</body>
</html>
```
____
### Now include sendMail view in urls.py existed in project folder
```python
from django.contrib import admin
from django.urls import path
from email_sending import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.sendMail, name = 'send_mail')
]
```
____
### Run the server and check in browser will get an form for entering details to send an email as shown below

![Github Image](email_form.JPG)
____
### Enter information to send an email in the form and click on send, So that your email is sent successfully

* Confirm that sender received an email along with attachment, Thus how we can send an email using Django. For more information about this email sending can check the below references given
____
### References

[Email Sending Using Django Reference Link](https://docs.djangoproject.com/en/3.0/topics/email/)






