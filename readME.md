#To learn list
* [x] task
* [x] task
* [ ] task

# Python
## Environment
```bash
#Create virtual environment
python3 -m venv .venv

#Activate
source .venv/bin/activate

#Select interpreter to match'which python' on the command line.
Python 3.11.1('venv': venv) ./venv/bin/python

#Leaving the virtual environment
deactivate

#Checking out
echo $VIRTUAL_ENV
which python

# Create or install requirements.txt
pip freeze > requirements.txt
pip install -r requirements.txt

```
## Flask
Files 
* create .git .venv .env .gitignore requirement.txt
* folder for styles.css, images
  
  `mkdir static/styles`
* folder for .html
  
    `mkdir static/styles`
  
```python
#without waitress
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/")
@app.route("/index")
def index():
    return "Hello World!"

#In development
if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8000,debug=True, use_reloader=True)

# In production
from waitress import serve
if __name__ == "__main__":
    serve(app, host="0.0.0.0", port=8000)
```


### Django
* Initialize new project
    ```bash
    django-admin  startproject myproject
    ```

* Quickly Start the server (dir to folder of myproject)
    ```bash
    cd myproject #dir to myproject
    python3 manage.py runserver 8000
    ```


* The top layer: myproject
    ```python
    #Create folder
    static (css/style.css and js/main.js)
    templates ( xx.html)
        # .html
        {% load static %}
        <link rel="stylesheet" href="{% static 'css/style.css' %}">
        <script src="{% static 'js/main.js' %}" defer></script>


    # myproject/myproject/views.py
    from django.shortcuts import render
    def homepage(request):
        return render(request,"home.html")

    def about(request):
        return render(request,"about.html")


    # myproject/myproject/urls.py  
    from django.contrib import admin
    from django.urls import path
    from . import views

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('',views.homepage),
        path('about/',views.about)
    ]


    # myproject/myproject/settings.py
    TEMPLATES = [{...,
        'DIRS': ['templates'], ...
    }]

    import os
    STATICFILES_DIRS =[
    os.path.join(BASE_DIR,'static')
    ]


    ```    

* New app (will see new forder named as 'posts')
    * Create App     dir: myproject/  
        `python3 manage.py startapp posts`

    * Register App   dir:myproject/myproject/settings.py  
        `INSTALLED_APPS = [ ..., 'posts' ]`
   
    * add urls.py  
        `myproject/posts/urls.py` 
        ```python
        from django.urls import path
        from . import views

        app_name = "posts"
        urlpatterns = [
            path('',views.posts_list, name='list'),
            path('<slug:slug>',views.post_page, name='page'),
            ]
        ```            
    * Registry new urls.py to original urls.py  
        `myproject/myproject/urls.py`
        ```
        ...
        from django.urls import path, include
        urlpatterns = [
            ...,
            path('posts/', include('posts.urls'))]
        ```
    * myproject/posts/views.py
        ```python
        def posts_list(request):
            return render(request,'posts/posts_list.html')
        ```

    * Create folder  
        `myproject/posts/templates `  

        `myproject/posts/templates/posts`      
        `myproject/posts/templates/posts/posts_list.html ` 




    


    

    # Add layout.html
    # myproject/templates/layout.html

    # refactor .html to take layout.html
        {% extend 'layout.html' %}

    
* Migratetion
    * create migration  
        `python3 manage.py makemigrations`
    * apply the migration to database  
        `python3 manage.py migrate`
    * ORM in the shell  
        `python3 manage.py shell`
        ```bash
        from posts.models import Post
        p = Post()
        p.title = "My first Post!!"
        p.save()
        Post.objects.all()
        exit()
        ```
    * Can do ORM in Admin  
        `python3 manage.py createsuperuser`  
        * Register your models in myproject/posts/admin.py  
            ```python  
            from .models import Post
            admin.site.register(Post) 
            ```

    * Displaying Posts
        * myproject/posts/views.py
            ```python
            from .models import Post
            posts = Post.objects.all()
            return render(request,'posts/posts_list.html', {'posts':posts})
            ```
        * templates/posts/.html  
            ```  ...```
        
    * use Named URL 
      * urls.py   
        `path('',views.posts_list, name='posts')`  
      * layout.html  
        ~~`<a href="/posts">📰</a>`~~  
        `<a href="{% url 'posts' %}">📰</a>|`
    
    * create pages for your app
        * urls.py  
            `path('<slug:slug>',views.post_page, name='page')`
        * views.py  
            ```python
            from django.http import HttpResponse
            def post_page(request, slug):
                return HttpResponse(slug)
            ```
        * posts_list.html  
            ~~`<h2>{{ post.title }}</h2>`~~  

            ```html
            <h2>
                <a href="{% url 'page' slug=post.slug %}">
                    {{ post.title }}
                </a>
            </h2>
        
        ```
    * Namespace registry
        * ~/posts/urls.py  ==> make these urlpatterns inside 'posts' app  
        `app_name = "posts"`
        * layout.html
            use namespace ...

* Upload & Display images
    * myproject/myproject/settings.py
        ```
        MEDIA_URL = 'media/'
        MEDIA_ROOT = os.path.join(BASE_DIR, 'media)
        ```
    * myproject/myproject/urls.py 
      ```python
        from django.conf.urls.static import static
        from django.conf import settings
        urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
        ```
    * `pip install Pillow`
    * myprojects/posts/models.py  
        ```python
        banner = models.ImageField(default = 'fallback.png', blank=True)
        ```
    * make migration after change models.py  
        `python3 manage.py makemigrations`
    * apply migration to database  
        `python3 manage.py migrate`
    * post_page.html
        ```python
        <img
            class="banner"
            src="{{ post.banner.url }}"
            alt = "{{ post.title }}"
        />
        ```
    

* Start the server (dir to folder of PROJECTNAME)
    ```bash
    python3 manage.py runserver 8000
    ```


  



### dotenv & os & requests
```python
from dotenv import load_dotenv
load_dotenv()

import os 
API_KEY = os.getenv("API_KEY")

import requests
data = requests.get(url).json()
```
### Map&Filter with lambda
```python
fun1 = lambda num: num*num
fun2 = lambda num:num%2!=0
numbers= [1,2,3,4]

s2 = map(fun,numbers)
print(list(squared))  # [1, 4, 9, 16]

odd = map(fun2, numbers)  
print(  list(map(fun2, numbers)) )    # [True,False,True,False]
print(  list(filter(fun2, numbers)) )  # [1,3]
```



# Typescript
# Git
* configuration
    ```bash
    git config --global user.name "Ares Chang"
    git config --global user.email "ares0628@gmail.com"
    git config --global init.defaultBranch main
    git config credential.helper 'cache --timeout=3600'
    git config --global credential.helper 'cache --timeout=7200'
    ```
* VScode setting
    >File>Preferences>settings file:Exclude delete **/.git

* Using repo
    ```bash
    git clone https:// ... .git
    rm -rf .git
    ```
* initialization
    ```bash
    git init
    git status
    git add .   #add all files at once to stage
    git commit -m "first commit"
    or
    git commit -a -m "first commit" #add all files at once and commit
    ```
* send to Github
    ```bash
    git push -u origin main
    ```




# Terminal
```bash
# Disconnect a port ex:8000
fuser -k 8000/tcp
```





# Markdown
[Reference](https://learn.microsoft.com/en-us/azure/devops/project/wiki/markdown-guidance?view=azure-devops#add-mermaid-diagrams-to-a-wiki-page 'wiki')

*I'm Italian*  or _I'm Italian_

**I'm strong**

~~I'm missed~~

<!--Blockquote -->
> My way or High way
<!--Links-->
[I'm the link](https://www.youtube.com/watch?v=HUBNt18RFbo&ab_channel=TraversyMedia)

[I'm the link](https://www.youtube.com/watch?v=HUBNt18RFbo&ab_channel=TraversyMedia "Ares")

[go to test](test.md)

[go back to H1](#heading-1 "H1")
* item 1
* item 2
* item 3
  * item 3.1
    1. i
    2. ii

<!--inline code block-->
`<p> I'm paragraph </p>`
<!--images-->
![Makedown logo](https://pbs.twimg.com/media/GMTfaG7b0AAUSuT?format=jpg&name=medium)


```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```