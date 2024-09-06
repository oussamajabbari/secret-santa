

Install env
===========

1. Create venv

> python -m venv .venv

2. Activate venv

> python -m venv .venv

3. Install pip

> python -m pip install --upgrade pip

4. Install fastapi

> pip install "fastapi[standard]"

5. Install database

> sudo apt install mysql-server
> sudo mysql_secure_installation

6. Create database user:

See https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04-fr

7. Create table

CREATE TABLE participants (
    id int,
    name varchar(255)
);

Run the server
==============

fastapi dev main.py
