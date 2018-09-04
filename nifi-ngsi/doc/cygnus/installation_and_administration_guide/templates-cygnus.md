### Templates

In order to reduce the configuration complexity fo many processors and controllers,
Cygnus includes some templates. A Template is a way of combining these basic building blocks into larger building blocks. Once a DataFlow has been created, parts of it can be formed into a Template. This Template can then be dragged onto the canvas, or can be exported as an XML file and shared with others. Templates received from others can then be imported into an instance of NiFi and dragged into the canvas.

The templates provided in the Cygnus are:

* Orion-To-MySQL
* Orion-To-PostgreSQL
* Orion-TO-Mongo

You can access each template dragging the Template button to the canvas in the Cygnus user interface. Once you select 
the template that you need, all the processors and controllers with a basic configuration
will be put and displayed in the Cygnus canvas.

![cygnus-template1](../images/cygnus-template1.png) 

### Orion-To-MySQL

The Orion-To-Mysql template include some preconfigured processors and controllers:

* DBCPConnectionPool controller for setting up the connection of the processors and the MySQL database.
* ListenHTTP processor for receiving data from the Orion Context Broker.
* NGSIToMySQL processor for creating updating and ingesting data  to MYSQL
* Log Attribute processor for keeping a register of the notifications received in Cygnus.

Now you can go to the properties tab of each processor and edit their properties as your convenience.
Once you have set up you configuration please enable the DBCPConnectionPool controller as is explained in the [Cygnus GUI](./cygnus_gui.md)
and start the processors.

### Orion-To-PostgreSQL

The Orion-To-PostgreSQL template include some preconfigured processors and controllers:

* DBCPConnectionPool controller for setting up the connection of the processors and the PostgreSQL database.
* ListenHTTP processor for receiving data from the Orion Context Broker.
* NGSIToPostgreSQL processor for creating updating and ingesting data to PostgreSQL
* Log Attribute processor for keeping a register of the notifications received in Cygnus.

Now you can go to the properties tab of each processor and edit their properties as your convenience.
Once you have set up you configuration please enable the DBCPConnectionPool controller as is explained in the [Cygnus GUI](./cygnus_gui.md)
and start the processors.

### Orion-To-Mongo

The Orion-To-Mongo template include some preconfigured processors and controllers:

* ListenHTTP processor for receiving data from the Orion Context Broker.
* NGSIToMySQL processor for creating updating and ingesting data to Mongo
* Log Attribute processor for keeping a register of the notifications received in Cygnus.

Now you can go to the properties tab of each processor and edit their properties as your convenience.
Once you have set up you configuration please enable the DBCPConnectionPool controller as is explained in the [Cygnus GUI](./cygnus_gui.md)
and start the processors.