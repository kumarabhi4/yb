Find an appropriate starting point for docker here:
https://docs.docker.com/get-docker/
and for podman here:
https://podman.io/getting-started/installation
---
Follow the steps here to start running yugabyte on a local container:
https://download.yugabyte.com/#docker
(for podman, all commands simply replace “docker” with “podman”. For example, `docker
pull yugabytedb/yugabyte:latest` becomes `podman pull
yugabytedb/yugabyte:latest` )
---
Once the container is running, show the following information from inside the container:
The container is running a linux distribution. Use the `docker exec` command to start a bash
shell inside the container.
Show the command and output you used to find the following:
- CPU usage of the process “yb-tserver”
- Demonstrate what linux distribution is running in the container
- Find any networking information you can. If possible, describe the IP address and
subnet.If you think a package is not present, or a command should work but is not installed, you are
welcome to install any additional packages using the package manager appropriate to the
distro, and provide the appropriate command in your answers.
---
Now, enter the postgres SQL interface for yugabyte. As noted in the documentation on the first
step, the command should be:
docker exec -it yugabyte /home/yugabyte/bin/ysqlsh
Now, all postgres commands should succeed. Thus, the answers to the following questions may
be easiest to find in postgres documentation.
Set up the following:
Create a table called `customers` with the following schema:
ID serial primary key
name char(30), and cannot be empty
email varchar(50) and must be unique
customer_since date
Provide the command you used to create this table, and the output of a “describe”
operation on the table.
Insert data into the table so that the following statements exist for your database:
yugabyte=# select * from customers where name = 'yugabyte';
id |
name
|
email
| customer_since
----+--------------------------------+----------------------+----------------
82 | yugabyte
| support@yugabyte.com | 2020-08-31
(1 row)
yugabyte=# select count(*) from customers;
count
-------
5
(1 row)Provide the statements used to make the table include the data as shown above
Now, find the following:
- use the pg_stat_activity table to show the number of connections to the database
- Show the database timezone
Short answer section:
--------------------------
1) You decided to move an application to the cloud. You have a distributed database in
different regions of a single cloud (like AWS or GCP). In order to handle global traffic,
your database is located in the following three regions:
a) US-East
b) Europe - Frankfurt
c) Asia-Pacific - Sydney
Your application is able to be multi region, but today you’ve only deployed in US-East.
Your users are assigned the local distributed db location based on their region - that is,
users with a US address have data in US-east; users based in Europe have data in
Europe-Frankfurt; and users in Asia access their data in Asia-Pacific - Sydney.
However, your users in Asia and Europe complain that the app is slow - you test, and
you realize query latency to the database from the app is 100 ms from US-East to Europe, and
200ms from US-East to Asia, while a query to US-East reliably returns in 10 to 15ms
Walk through what steps you would take to resolve the latency issue for European and Asian
located users.
---------------------------2) Write a script which will tar all logs on a distributed system of n nodes, then create a
bundle of logs on the local system at “/tmp/[case-number]-logbundle-[today’s
date].tar.gz”.
You have the following constraints:
- Logs are present in the form “tserver-INFO-[date]-[PID].log” in “/home/yugabyte/logs”
- You need to collect up to 10 logs of a single PID, with the option to collect all of them.
That is, there may be 15 logs of PID 15664 but you should by default collect the most
recent 10, but make it easy to collect all logs should they be necessary.
-
There are other logs in the same folder like “tserver-WARNING” and
“mserver-WARNING” and “mserver-INFO” but only tserver-INFO logs should be
collected
- An ssh key for user “yugabyte” is present at `.ssh/id_rsa_yugabyte.pub`
- A list of nodes is present in `/tmp/nodes` in the form:
hostname1:ssh-port
hostname2:ssh-port
-
You may use any scripting language which may be editable at the customer location.
-------------------------------3)
You have the above network diagram.
a) Computer A and Computer B can talk to each other via IP. Computer C cannot talk to
computers A or B. What could be some reasons why Computer C cannot talk to
computers A or B?
b) How can you test if computer A has network connectivity to computer D?
c) Assume Computer A and D cannot communicate with one another. What could be some
reasons why?
d) An application on Computer D can send data to a server on Computer A, but cannot
receive a return packet from Computer A. What could cause this problem?4) You need to design a schema for a “users” table that ingests data from a web application.
You’re hoping to scale to millions of users.
Your users need to be able to look up their accounts using either “email” or “username”. You
also include phone number, First Name, Last Name, Role (eg Admin, Standard, Read-Only),
Address, Country, Date User added, and Last login time. You’re not sure about how well your
application team is doing data validation.
Provide a sample Schema.
a) Indicate which columns should have indexes, if any
b) Do you recommend there should be any constraints on the data? If so, what constraints
should exist on which columns?
c) If you are deploying this schema on a distributed database, which column(s) might be a
recommended choice for your distribution key? Which columns would be poor choices
for distribution keys? What symptoms might I expect to see if I put a distribution key on
“Country”?
