**Table Of Contents:**
--------------------------------------------------------------------------------------
### [Why Python 3.11.x and not Python 3.12.x?](Why-Python-3.11.x-and-not-Python-3.12.x?)
### [Why Python Peotry and not VEVN, etc?](Why-Python-Peotry-and-not-VEVN?)
### [Why Litestar and not FastAPI or Starlette?](Why-Litestar-and-not-FastAPI-or-Starlette?)

<a id="Why-Python-3.11.x-and-not-Python-3.12.x?"></a>
## Why Python 3.11.x and not Python 3.12.x?
- There are some changes in Python 3.12.x which I am not a big fan of such as not including setuptools by default (https://github.com/python/cpython/issues/95299).
- Plus some third party libaries may not be 100% compatable with 3.12.x yet.
- That said, I'm pretty sure that everything in this series of articles will work as expected.

<a id="Why-Python-Peotry-and-not-VEVN?"></a>
## Why Python Peotry and not VEVN, etc? ##
On most systems when you install Python, you also install PIP (https://en.wikipedia.org/wiki/Pip_(package_manager)) which helps you by installing dependant libraries.
PIP works fine most of the time, but there can be instantances where it may install newer libraries which are incompatible with your setup.

***Consider the following:***
#### **Using PIP**
- Your project depends on `Library1 1.4`
- `Library1 1.4` depends on `Sub-Library 2.9`
Only `Library1 1.4` is writen to your requirements file.

If `Sub-Library 2.9` gets a version bump to 3.0 and it has a documentated breaking change then `Library1 1.4` stops working as expected.

When `Library1 1.4` stops working you get a new error message saying something about an incompability in Library1.
However, you are still usnig `Library1 1.4`.

You cannot force Library1 to use `Sub-Library 2.9` and Library1 has not been updated in six months. Now you are dependant the Library1 maintainer or Sub-Library 2.9 maintainer to fix the issue. If the Library1 maintainer simply correctly defined the projects dependancies this would not be an issue. However, people make mistakes and other things happen.

#### **Using [Poetry](https://python-poetry.org/) - https://python-poetry.org/**
- Your project depends on `Library1 1.4`
- `Library1 1.4` depends on `Sub-Library 2.9`

All libraries and thier dependant libraries names and hashes are written to a dictionary file when the lookup file is edited. With the file names and hashes for Poetry to referance when needed it does not matter if a down stream library is updated.

**Okay, but what about the virtual environment part?**

Poetry includes a virtual environment which works as well as VENV or any other Python virtual environment, however it uses the Poetry lookup/dictionary files to handle your Python dependancies.

<a id="Why-Litestar-and-not-FastAPI-or-Starlette?"></a>
## Why Litestar and not FastAPI or Starlette? ##
FastAPI is built on top of Starlette and/or use the same libraries as Starlette. 
FastAPI includes some nice features like automatic OpenAPI generation, Pydantic which are not found in Starlette.<br>
However, [this](https://medium.com/@v3ss0n/litestar-2-0-a-faster-proper-fastapi-alternative-is-launching-soon-cf543a0931f8) article by <cite>[Phyo Arkar Lwin](https://medium.com/@v3ss0n)</cite> lists some, of what I think are, compelling reasons to use Litestar instead of FastAPI. The main points listed include:
- performance
- speed of issues resolved and integerated in Github
- direct support on discord (https://discord.gg/aJUvZRse)
- built-in [htmx](https://htmx.org/) - https://htmx.org/ support

Other reasons include:
- routes can be handled by `class controller`s
  - allows for a set of endpoints to be programmed in one Python class somewhat like C#
- alterative/additional OpenAPI interfaces

## Begining Steps ##
First take some time to go over the four examples found on [Litestar Docs => Tutorials](https://docs.litestar.dev/latest/tutorials/index.html) - https://docs.litestar.dev/latest/tutorials/index.html

1. [Developing a basic TODO application](https://docs.litestar.dev/latest/tutorials/todo-app/index.html">) - https://docs.litestar.dev/latest/tutorials/todo-app/index.html<br>
This tutorial is intended to familiarize you with the basic concepts of Litestar. If you have no prior experience with Litestar or web frameworks in general, this is the right place to start.
2. [Improving the TODO app with SQLAlchemy](https://docs.litestar.dev/latest/tutorials/sqlalchemy/index.html) - https://docs.litestar.dev/latest/tutorials/sqlalchemy/index.html<br>This tutorial is aimed at developers who are already familiar with Litestar’s core concepts such as route handlers and dependency injection.
3. [Data Transfer Object Tutorial](https://docs.litestar.dev/latest/tutorials/dto-tutorial/index.html) - https://docs.litestar.dev/latest/tutorials/dto-tutorial/index.html<br>
This tutorial is intended to familiarize you with the basic concepts of Litestar’s Data Transfer Objects (DTOs). It is assumed that you are already familiar with Litestar and fundamental concepts such as route handlers.
4. [SQLAlchemy Repository Tutorial](https://docs.litestar.dev/latest/tutorials/repository-tutorial/index.html) - https://docs.litestar.dev/latest/tutorials/repository-tutorial/index.html<br>This tutorial covers Litestar’s SQLAlchemy repository. It assumes an understanding of Litestar’s key concepts, as well as a degree of familiarity with SQLAlchemy.


## Sample Project (Publishers And Books) ##
To keep things simple, I'll keep each entity to it's minimum.
### Entities: ###
  - **Publisher**
    - name (string)
  - **Book**
    - name (string)

Each entity will also contain a primary key and a field to sort the queries by.
A publisher can have zero or more books.<br>
Therefore we end up with the following entities:
<table>
    <tr valign="top">
        <td>
            <table border="1">
                <caption>Publisher</caption>
                <tr valign="top">
                    <td>id</td>
                    <td>integer - primary key<br><small><em>auto increment</em></small></td>
                <tr>
                <tr><td>name</td><td>string</td></tr>
                <tr><td>sort_order</td><td>integer</td></tr>
            </table>
        </td>
        <td>
            <table border="1">
                <caption>Book</caption>
                <tr valign="top">
                    <td>id</td>
                    <td>integer - primary key<br><small><em>auto increment</em></small></td>
                <tr>
                <tr><td>name</td><td>string</td></tr>
                <tr><td>sort_order</td><td>integer</td></tr>
                <tr><td>publisher_id</td><td>integer - foreign key</td></tr>
            </table>
        </td>
    </tr>
</table>