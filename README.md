QTOJ: Quang Tri Online Judge
===

## Overview

Homepage: [https://quangtroj.edu.vn](https://quangtroj.edu.vn)

Based on [LQDOJ](https://lqdoj.edu.vn/).

Supported languages: 

- Assembly (x64)
- AWK
- Brainfxck
- C
- C++03 / C++11 / C++14 / C++17 / C++20
- Java 8 / Java 11
- Scratch
- Pascal
- Perl
- Python 2 / Python 3
- PyPy 2 / PyPy 3

Support plagiarism detection via [Stanford MOSS](https://theory.stanford.edu/~aiken/moss/).

## Installation

Most of the setup are the same as DMOJ installations. You can view the installation guide of DMOJ here: https://docs.dmoj.ca/#/site/installation.
There is one minor change: Instead of `git clone https://github.com/DMOJ/site.git`, you clone this repo `git clone https://github.com/TLEJudge/online-judge.git`.

### Additional Steps in Production:

1. To use newsletter (email sending), go to admin and create a newsletter.
2. Change the domain name and website name in Admin page: Navigation Bars/Sites

### Some frequent difficulties when installation:

1. Missing the `local_settings.py`. You need to copy the `local_settings.py` in order to pass the check.
2. Missing the problem folder in `local_settings.py`. You need to create a folder to contain all problem packages and configure in `local_settings.py`.
3. Missing static folder in `local_settings.py`. Similar to problem folder, make sure to configure `STATIC_FILES` inside `local_settings.py`. 
4. Missing configure file for judges. Each judge must have a seperate configure file. To create this file, you can run `python dmojauto-conf`. Checkout all sample files here https://github.com/DMOJ/docs/blob/master/sample_files.

## Usage

Suppose you finished all the installation. Everytime you want to run a local server, follow these steps:

1. Activate virtualenv:
```bash
source dmojsite/bin/activate # Should replace your default ~/.bashrc
```

2. Run server:
```bash
python manage.py runserver 0.0.0.0:80 #80 is a good idea? :kekw:
```

3. Create a bridge (this is opened in a different terminal with the second step if you are using the same machine)
```bash
python manage.py runbridged # Use a tmux session, then detach it (I recommend doing so)
```

4. Create a judge (another terminal)
```bash
dmoj 0.0.0.0 -p 9999 -c <path to yml configure file> # You may not have to do this
```
Here we suppose you use the default port 9999 for bridge in `settings.py`. You can create multiple judges, each should be in a seperate terminal.

**Optional**

5. Run celery worker (This is server's queue. It may be necessary in some functions)
```bash
celery -A dmoj_celery worker
```

6. Run a live event server (So everything is updated lively like in the production)
```bash
node websocket/daemon.js
```

## Deploy
Most of the steps are similar to LQDOJ Docs. Here are two steps:

1. Update vietnamese translation:
 - If you add any new phrases in the code, ```python manage.py makemessages```
 - go to `locale/vi`
 - modify `.po` file
 - ```python manage.py compilemessages```
 - ```python manage.py compilejsi18n```

2. Update styles (using SASS)
 - Change .css/.scss files in `resources` folder
 - ```./make_style && python manage.py collectstatic```
 - Sometimes you need to `Ctrl + F5` to see the new user interface in browser.
