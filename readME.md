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