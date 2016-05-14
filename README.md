# Server configration

***

## Enviroments

1. mysql
    ```
    sudo apt-get install mysql-server mysql-client libmysqlclient-dev
    
    service mysql start
    sudo netstat -tap | grep mysql
    mysql -uroot -p
	
	sudo vim /etc/mysql/my.cnf
	
	# switch off case sensitive for importing from win
	[mysqld]
	lower_case_table_names = 1
    ```
2. apache
    ```
    sudo apt-get install apache2
    
    # python mod-wsgi
    sudo apt-get install libapache2-mod-wsgi
    sudo vim /etc/apache2/sites-available/site.conf
    ```
    ```apacheconf
    # apache django
    <VirtualHost *:80>
        ServerName djangoapp.domain.com
        # ServerAlias domain.com
        ServerAdmin hevlhayt@foxmail.com
    
        Alias /static/ /home/user/project/static/
    
        <Directory /home/user/project/static>
            Require all granted
        </Directory>
		
		WSGIDaemonProcess djangoapp python-path=/home/user/projects/DjangoApp:/home/user/projects/DjangoApp/venv/lib/python2.7/site-packages processes=2 threads=15 display-name=%{GLOBAL}
		WSGIProcessGroup djangoapp
        WSGIScriptAlias / /home/user/projects/DjangoApp/wsgi.py
    
        <Directory /home/user/projects/DjangoApp>
        <Files wsgi.py>
            Require all granted
        </Files>
        </Directory>
    </VirtualHost>
	
	# apache flask
	<VirtualHost *:80>
		ServerName flaskapp.domain.com

		WSGIDaemonProcess flaskapp python-path=/home/user/projects/FlaskApp:/home/user/projects/FlaskApp/venv/lib/python2.7/site-packages processes=2 threads=15 display-name=%{GLOBAL}
		WSGIScriptAlias / /home/user/projects/FlaskApp/app.wsgi

		<Directory /home/user/projects/FlaskApp>
			WSGIProcessGroup flaskapp
			WSGIApplicationGroup %{GLOBAL}
			Require all granted
		</Directory>
	</VirtualHost>

    ```
    ```
    sudo a2ensite site
    service apache2 reload
    cat /var/log/apache2/error.log
    ```
3. python
    ```
    sudo apt-get install python-pip
    sudo apt-get install python-dev
	
	sudo pip install MySQL-python
    sudo pip install sqlalchemy
    ```
    
4. django
    ```
	sudo pip install django
	
    # sync django
    python manage.py makemigrations
    python manage.py migrate
    ```
    ``` python
    # wsgi.py
    import os
    from os.path import dirname, abspath
    
    PROJECT_DIR = dirname(dirname(abspath(__file__)))
    
    import sys
    sys.path.insert(0, PROJECT_DIR)
    
    os.environ["DJANGO_SETTINGS_MODULE"] = "DjangoApp.settings"
    
    from django.core.wsgi import get_wsgi_application
    application = get_wsgi_application()
    ```
    
    ```
    # user uploading path
    chown -R www-data:www-data /upload
    sudo chmod -R g+w /upload
    ```
5. flask
	```
	sudo pip install Flask
	
	sudo apt-get install python-virtualenv
	cd project
	virtualenv venv
	```
	``` python
	# app.wsgi
	activate_this = '/home/user/projects/FlaskApp/venv/bin/activate_this.py'
	execfile(activate_this, dict(__file__=activate_this))

	from yourapp import app as application

	import sys
	sys.path.insert(0, '/home/user/projects/FlaskApp')
	```
	
6. git
	```
	git rm -r --cached .
	git add .
	git commit -m ".gitignore update"
	```
    
> If you have any problem, please contact hevlhayt@foxmail.com (ﾉﾟ▽ﾟ)ﾉ