Title: Django Request Response processing
Date: 2009-06-24 23:29:56
Modified: 2009-06-25 09:59:56+05:30
Slug: django-request-response-processing
Authors: shabda
Tags: django
Summary: Have you wondered the steps a users request goes through before being responded to by Django? The answer lies in reading `django.core.handlers.base` and `django.core.handlers.wsgi`. Here is a diagram explaining what happens. ([Click to enlarge](http://uswaretech.com/blog/wp-content/uploads/2009/06/django_request_response_lifecycle.png).) <a href="http://uswaretech.com/blog/wp-content/uploads/2009/06/django_request_response_lifecycle.png"><img src="http://uswaretech.com/blog/wp-content/uploads/2009/06/django_request_response_lifecycle-300x207.png" alt="" title="django_request_response_lifecycle" width="300" height="207" class="alignnone size-medium wp-image-587" /></a> The steps are. (With Apache/Mod_wsgi, similar steps for other setup.) 1. User's request comes to Apache etc. 2. Apache sends request to `django.core.handlers.wsgi` via `mod_wsgi`. 3. A list of request and response middleware callables is created. 4. Request middleware is applied. If it sends a response, it is returned to the user. 5. `urlresolvers.resolve` finds
Have you wondered the steps a users request goes through before being responded to by Django? The answer lies in reading `django.core.handlers.base` and `django.core.handlers.wsgi`. Here is a diagram explaining what happens. ([Click to enlarge](http://uswaretech.com/blog/wp-content/uploads/2009/06/django_request_response_lifecycle.png).)

<a href="http://uswaretech.com/blog/wp-content/uploads/2009/06/django_request_response_lifecycle.png"><img src="http://uswaretech.com/blog/wp-content/uploads/2009/06/django_request_response_lifecycle-300x207.png" alt="" title="django_request_response_lifecycle" width="300" height="207" class="alignnone size-medium wp-image-587" /></a>

The steps are.
(With Apache/Mod_wsgi, similar steps for other setup.)

1. User's request comes to Apache etc.
2. Apache sends request to `django.core.handlers.wsgi` via `mod_wsgi`.
3. A list of request and response middleware callables is created.
4. Request middleware is applied. If it sends a response, it is returned to the user.
5. `urlresolvers.resolve` finds the view funcion to use.
6. View middleware is applied. If response comes, it is sent back to the user.
7. View function is called. It talks to models to do business logic, and renders the templates.
8. The response middleware is applied, and response is sent back to the users.

This misses a lot of important steps (Exception middleware, request_context populating, ...) but is a basic high level overview. 

Resources

* [django/core/handlers/base.py](http://code.djangoproject.com/browser/django/trunk/django/core/handlers/base.py)
* [django/core/handlers/wsgi.py](http://code.djangoproject.com/browser/django/trunk/django/core/handlers/wsgi.py)
* [.dia and other files for the image](http://github.com/uswaretech/Django-Request-Response/tree/master)

---------------------
Do you [twitter](http://twitter.com/uswaretech)? Do you [Github](http://github.com/uswaretech)? Find us there.


