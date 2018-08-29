# Cygnus Quick Start Guide
This quick start overviews the steps a newbie programmer will have to follow in order to get familiar with Cygnus and its basic functionality. For more detailed information, please refer to the [README](https://github.com/telefonicaid/fiware-cygnus/blob/master/README.md); the [Installation and Administration Guide](./installation_and_administration_guide/introduction.md), the [User and Programmer Guide](user_and_programmer_guide/introduction.md) and the [Processors Catalogue](flume_extensions_catalogue/introduction.md) fully document Cygnus.

## Scenario
The scenario presented in this guide is composed by two containers.
One for running Cygnus and other for running MYSQL
![start-scenario](./images/scenario.png)
## Running containers 
The aim of this guide is providing an easy guide to setup a setup cygnus and other containers
for persisting context data:

In this guide we will run a basic example of cygnus for 
persisting NGSIv2 events to MySQL. As a prerequisite you need to have already have installed Docker and Docker compose.

## <a name="section1"></a>Before starting
Obviously, you will need docker and docker-compose installed and running in you machine. Please, check [this](https://docs.docker.com/linux/started/) official start guide.

[Top](#top)

## <a name="section2"></a>Getting an image
### <a name="section2.1"></a>Building from sources
(1) Start by cloning the `fiware-cygnus` repository:

    $ git clone https://github.com/ging/fiware-cygnus.git
    $ https://github.com/anmunoz/PrbAndres.git
    $ cd PrbAndres
    $ git checkout cygnus

Change directory:

    $ cd nifi-ngsi/docker

And run the following command:

    $ sudo docker-compose up .

Once finished (it may take a while), you can check the available images at your docker by typing:

```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
cygnus              latest              6a9e16550c82        10 seconds ago      462.1 MB
mysql               latest              273a1eca2d3a        2 weeks ago         194.6 MB
```

[Top](#top)

(2) Once you have your containers up and running, you can add
the template provided for persisting data to MySQl.

First, open you browser and open NiFi http://127.0.0.1:9090/nifi

Now go to the Components toolbar that is placed in the upper section
of the NiFi GUI.

![cygnus-gui](./images/nifi-toolbar-components.png)

Drag and drop the template button and select the template Orion-To-Mysql
this template contains two processors. The first processor opens a 
connection for getting NGSIv2 notifications through the 5050 port. On the other hand
the second processor called NGSIToMySQL is in charge to get the NGSIv2 events and 
persist that data into the MySQL database.

Before starting the processors you need to set your mysql password
and enable the DBCPoll controller. For doing that please follow the instructions :
* 1 > Do right click to any part of the NiFI GUI user space and then click on configure, 
a list of a controllers should be displayed
* 2 > Click on the configuration button of the "DBCPConnectionPool"
* 3 > Put "example" in the password field
* 4 > Save and Enable the controller clicking in the thunder icon placed on
the right part of the controller options.

At this point you are able to select all the processors and start them.

With all the configuration done now yuo can test Cygnus sending a notification to the 
listening port of NiFi  in our case is "5050" and the path "v2/notify".

Now for test your deployment, you may send a NGSI-like notification emulation (please, check the notification examples at [cygnus-ngsi](cygnus-ngsi/resources/ngsi-examples)):


(3) Open a new terminal (since Cygnus should be printing logs on the standard output of the first one) and create and edit somewhere a `notification.sh` file:

```
URL=$1

curl $URL -v -s -S --header 'Content-Type: application/json; charset=utf-8' --header 'Accept: application/json' --header "Fiware-Service: qsg" --header "Fiware-ServicePath: test" -d @- <<EOF
{
	"subscriptionId": "51c0ac9ed714fb3b37d7d5a8",
	"data": [{
		"temperature": {
			"type": "Float",
			"value": 30.73,
			"metadata": {}
		},
		"type": "Room",
		"id": "Room1"
	}]
}
EOF
```

This script will emulate the sending of an Orion notification to the URL endpoint passed as argument. The above notification is about and entity named `Room1` of type `Room` belonging to the FIWARE service `qsg` and the FIWARE service path `test`; it has a single attribute named `temperature` of type `float`.

(4) Give execution permissions to `notification.sh` and run it, passing as argument the URL of the listening `HTTPSource`:

```
$ chmod a+x notification.sh
$ ./notification.sh http://localhost:5050/v2/notify
```

(5) Look at the provenance report in the log attribute processor.

![cygnus-provenance](./images/nifi-provenance.png)

(6) You can check if the database and the table has been created. First enter 
to the MySQL container console 
```
sudo docker exec -it mysql /bin/bash

```
Then check the created databases and his tables. For logging to mysql 
use "root" as user and "example" as password
```
$ mysql -u root -p
mysql> show databases;
```
output:
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| qsg                |
| sys                |
+--------------------+
5 rows in set (0.06 sec)

```

```
mysql> use qsg;
mysql> show tables;
```
output:
```
+---------------+
| Tables_in_qsg |
+---------------+
| test          |
+---------------+
1 row in set (0.09 sec)

```

```
mysql> select * from test;

```
output:
```
+---------------+---------------------+-------------------+----------+------------+-------------+----------+-----------+--------+
| recvTimeTs    | recvTime            | fiwareServicePath | entityId | entityType | attrName    | attrType | attrValue | attrMd |
+---------------+---------------------+-------------------+----------+------------+-------------+----------+-----------+--------+
| 1535550393717 | 08/29/2018 13:46:33 | test              | Room1    | Room       | temperature | Float    | 30.73     | []     |
+---------------+---------------------+-------------------+----------+------------+-------------+----------+-----------+--------+
1 row in set (0.05 sec)

```

Now you can receive NGSIv2 notification from orion context broker and
store the data into MySQL using NiFi.

## Reporting issues and contact information
There are several channels suited for reporting issues and asking for doubts in general. Each one depends on the nature of the question:

* Use [stackoverflow.com](http://stackoverflow.com) for specific questions about this software. Typically, these will be related to installation problems, errors and bugs. Development questions when forking the code are welcome as well. Use the `fiware-cygnus` tag.
* Use [ask.fiware.org](https://ask.fiware.org/questions/) for general questions about FIWARE, e.g. how many cities are using FIWARE, how can I join the accelarator program, etc. Even for general questions about this software, for instance, use cases or architectures you want to discuss.
* Personal email:
    * [jamunoz@dit.upm.es](mailto:jamunoz@dit.upm.es) **[Main contributor]**
    * [jsalvachua@upm.es](mailto:jsalvachua@upm.es) **[Contributor]**
    
**NOTE**: Please try to avoid personaly emailing the contributors unless they ask for it. In fact, if you send a private email you will probably receive an automatic response enforcing you to use [stackoverflow.com](http://stackoverflow.com) or [ask.fiware.org](https://ask.fiware.org/questions/). This is because using the mentioned methods will create a public database of knowledge that can be useful for future users; private email is just private and cannot be shared.
