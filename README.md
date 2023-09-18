# postgres-Sql
	
PostgreSQL is a powerful, open source object-relational database system that uses and extends the SQL language combined with many features that safely store and scale the most complicated data workloads. The origins of PostgreSQL date back to 1986 as part of the POSTGRES project at the University of California at Berkeley and has more than 30 years of active development on the core platform.

PostgreSQL has earned a strong reputation for its proven architecture, reliability, data integrity, robust feature set, extensibility, and the dedication of the open source community behind the software to consistently deliver performant and innovative solutions. PostgreSQL runs on all major operating systems, has been ACID-compliant since 2001, and has powerful add-ons such as the popular PostGIS geospatial database extender. It is no surprise that PostgreSQL has become the open source relational database of choice for many people and organisations.

Getting started with using PostgreSQL has never been easier - pick a project you want to build, and let PostgreSQL safely and robustly store your data."	

		
### On Linux, SonarQube only works with PostgreSQL, which means we have to take a few extra steps to get it installed. First, download the PostgreSQL GPG key with the command	
	wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -	
		
### Next, add the PostgreSQL apt repository by running the command	
	sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ lsb_release -cs-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'	
		
### Update apt with the command	
	sudo apt-get update	
		
### Install PostgreSQL with by issuing	
	sudo apt install postgresql postgresql-contrib -y	
		
### start the PostgreSQL service	
	sudo systemctl start postgresql	sudo systemctl restart postgresql
		
### Enable the servic	
	sudo systemctl enable  postgresql	
		
### Now we must set a password for the PostgreSQL user with the command	
	sudo passwd postgres	
		
### Switch to the postgres user with the command	
	su - postgres	
		
### Let’s create a new database user	
	createuser sonar	
	create user msf with encrypted password 'ibos@123';
		
### We can now create our database. To do so, first log into the PostgreSQL console	
	psql	
		
### Set a password for the sonar user with the command	
	ALTER USER sonar WITH ENCRYPTED PASSWORD 'PWORD';	
		
### Where PWORD is a strong/unique password.	
	
### Create the SonarQube database with the command:	
	CREATE DATABASE sonarqube OWNER sonar;	
		
### Modify the privileges, such that the sonar user can access/use/modify the data with the command	
	GRANT ALL PRIVILEGES ON DATABASE sonarqube to sonar;	
		
### Exit the database console with the command:	
	\q	
		
		
# Install PostgreSQL 13		
		
		
### Step 1: Update Ubuntu system	
	sudo apt update
	sudo apt -y upgrade	
		
### Reboot system	
	sudo reboot	
		
### Step 2: Add PostgreSQL 13 repository to Ubuntu 20.04 | 18.04	
	sudo apt -y install vim bash-completion wget wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -"	
		
### After importing GPG key, add repository contents to your Ubuntu 20.04|18.04 system:
	echo ""deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main"" |sudo tee  /etc/apt/sources.list.d/pgdg.list
	
#### [ The repository added contains many different packages including third party addons. They include:

>> postgresql-client
>> postgresql
>> libpq-dev
>> postgresql-server-dev
>> pgadmin packages
# ]
		
### Step 3: Install PostgreSQL 13 on Ubuntu 20.04/18.04 Linux	
	sudo apt install postgresql-13 postgresql-client-13	
	sudo apt update
		
### The PostgreSQL service is started and set to come up after every system reboot.(see the status)
	systemctl status postgresql.service
		
	systemctl status postgresql@13-main.service	
		
	systemctl is-enabled postgresql	Enable
		
### Step 4: Test PostgreSQL Connection	
	sudo su - postgres	
		
### Let’s reset this user password to a strong Password we can remember.
	psql -c "alter user postgres with password 'StrongAdminP@ssw0rd'
	
### Start PostgreSQL prompt by using the command:	
	psql	
		
### Get connection details like below.	
   postgres=# \conninfo	
		
## Let’s create a test database and user to see if it’s working.		
		
### List created databases:	
  postgres=# CREATE DATABASE mytestdb;	
		
### Let’s create a test database and user to see if it’s working.
  postgres=# CREATE USER mytestuser WITH ENCRYPTED PASSWORD 'MyStr0ngP@SS';	

### Let’s create a test database and user to see if it’s working.(CREATE ROLE)
  postgres=# GRANT ALL PRIVILEGES ON DATABASE mytestdb to mytestuser;	

### List created databases(GRANT)
  postgres=# \l	
		
### Connect to database:	
  postgres-# \c mytestdb	
		
### Step 5: Configure remote Connection (Optional)
#### [ Installation of PostgreSQL 13 on Ubuntu only accepts connections from localhost. In ideal production environments, you’ll have a central database server and remote clients connecting to it – But of course within a private network (LAN).]	
		
### To enable remote connections, edit PostgreSQL configuration file:
    sudo nano /etc/postgresql/13/main/postgresql.conf 
### Uncomment line 59 and change the Listen address to accept connections within your networks.
[
# Listen on all interfaces
listen_addresses = '*'

# Listen on specified private IP address
listen_addresses = '192.168.10.11'
]
		
### Also set PostgreSQL to accept remote connections from allowed hosts.
	sudo nano /etc/postgresql/13/main/pg_hba.conf
[
# Accept from anywhere
host all all 0.0.0.0/0 md5

# Accept from trusted subnet
host all all 10.10.10.0/24 md5
]
		
### After the change, restart postgresql service.
    sudo systemctl restart postgresql
	
### Confirm Listening addresses.
    netstat  -tunelp | grep 5432	
