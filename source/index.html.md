---
title: Wevolver API Reference
language_tabs:
  - python
---

# Introduction

Welcome to the *Groot* API! Using *Groot* you'll be able to interact with a git backend ( self-hosted or local ) to create and manipulate repositories.

```
API Endpoint
http://localhost:8000

Summary of Resources:
/create/username/projectname
/raw/username/projectname
/username/projectname/delete
/username/projectname?path=
/username/projectname/listbom
/username/projectname/newfolder
/username/projectname/download?path=
/username/projectname/upload
/username/projectname/archive
/username/projectname/archive/download
```
# Authentication

Authentication is disabled when Django's debug setting is set to True. When Authentication is enabled
you will not be able to use any of the endpoints.

# Usage

Run *Groot*

```bash
python manage.py runserver
```

Then either interact programmatically using the examples in this document, or use Postman
and *Groot's* Postman collection

# Endpoints

## Get Raw File

Gets the raw contents of the file at the specified path.

```python
python import requests
requests.get("/raw/<username>/<projectname>?path=readme.md")
```

> The above command returns JSON structured like this:

```markdown
#projectname
This is where you should document your project  
### Getting Started
```

### HTTP Request
`GET /raw/<username>/<projectname>?path=readme.md`

## Delete Project

Deletes the specified project.

```python
python import requests
requests.get("/<username>/<projectname>/delete")
```

> The above command returns a message structured like this:

```json
Deleted at ./repos/username/projectname
```

### HTTP Request

`GET /<username>/<projectname>/delete`

## Download Archive

Downloads a zipped archive of the whole project.

```python
python import requests
```

> The above command returns JSON structured like this:

### HTTP Request

`LOCK /<username>/<projectname>/archive/download`

## Upload File

Uploads the attached file to the specified path.

```python
python import requests
```

> The above command returns JSON structured like this:

### HTTP Request

`LOCK /<username>/<projectname>/upload`

## Download File

Returns the contents of the file at the specified path.

```python
python import requests
requests.get("/<username>/<projectname>/download?path=readme.md")
```

> The above command returns JSON structured like this:

```markdown
#mytest
This is where you should document your project  
### Getting Started
```
### HTTP Request

`GET /<username>/<projectname>/download?path=readme.md`

## Create Folder

Creates a new folder in the given directory. The folder path is specified by the
path parameter of the POST.

```python
python import requests
requests.post("/<username>/<projectname>/newfolder")
```

> The above command returns JSON structured like this:

```json
{
    "message": "Folder Created"
}
```

### HTTP Request

`POST /<username>/<projectname>/newfolder`

## List Full Bom

Looks through the entire project for any file named `bom.csv`. Each of these
files are concatenated and, once duplicates are removed, the master BOM is returned.

```python
python import requests
requests.get("/<username>/<projectname>/listbom")
```

> The above command returns JSON structured like this:

### HTTP Request

`GET /<username>/<projectname>/listbom`

## Get File Tree

Grabs and returns a single file or a tree from a user's repository

if the requested object is a tree the function parses it instead
of returning blindly.

**This naming makes no sense**

```python
python import requests
requests.get("/<username>/<projectname>?path=/")
```

> The above command returns JSON structured like this:

```json
{
    "file": null,
    "tree": {
        "data": [
            {
                "name": "new",
                "type": "tree",
                "oid": "7b93b85c3a2aba0d510cc223ba15501b8c6283d5"
            },
            {
                "name": "readme.md",
                "type": "blob",
                "oid": "63c3ffbf73a78311a168a6949ec6bce690faa74e"
            }
        ]
    }
}
```

### HTTP Request

`GET /<username>/<projectname>?path=/`

## Create Project

Creates a bare repository with the provided username and projectname.
The directory structure is generated uniquely using a hash of the username.
See: [here](https://github.com/blog/117-scaling-lesson-23742)

```python
python import requests
requests.get("/create/<username>/<projectname>")
```

> The above command returns JSON structured like this:

```json
Created at ./repos/username/projectname
```
### HTTP Request

 `GET /create/<username>/<projectname>`
