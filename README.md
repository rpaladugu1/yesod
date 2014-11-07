Yesod Web Framework with Postgres Database option

Docker Yesod Web Framework from Stackage with postgres database option.

How about get going with Yesod Web Framework with Postgres option within few minutues?

With few steps you can be up and running with Yesod Web Framework. Just follow the following steps:

Step1:

sudo docker run --name postgresql -p 5432:5432 -v /vagrant/data -d postgres

This is will download and start a postgres process in the background with data linked to your data folder in home directory with port 5432 exposed.

Check with : sudo docker ps

Step2: To use psql and to create necessary user id and project database

sudo docker run -it --link postgresql:postgres --rm postgres sh -c 'exec psql -h $POSTGRES_PORT_5432_TCP_ADDR -p $POSTGRES_PORT_5432_TCP_PORT -U postgres'

then issue the following commands to create the userid and database for your yesod project

CREATE USER project1 WITH PASSWORD 'project1';
CREATE DATABASE project1 WITH OWNER project1;

Step 3: Now, go ahead and create a Yesod project. Calling it “project1”

sudo docker run --rm -t -i --link postgresql:postgres  --volumes-from postgresql  -v /vagrant/yesodprojects/project1:/var/yesodprojects/project1 rpaladugu1/yesod-postgresql /bin/bash -c "printf %s\\\n 'project1' 'p' | yesod init && cd project1 && cabal sandbox init --sandbox=/var/yesodprojects/.cabal-sandbox"

Step 4: Update the postgres.yml file in the config folder of yesod project

first note down the value of POSTGRES_PORT_5432_TCP_ADDR variable i.e. ip address of postgres host

sudo docker run --rm -t -i --link postgresql:postgres  --volumes-from postgresql -v /vagrant/yesodprojects/project1:/var/yesodprojects/project1 rpaladugu1/yesod-postgresql /bin/bash -c "env | grep POSTGRES_PORT_5432_TCP_ADDR "

go to /vagrant/yesodprojects/project1/config folder , edit postgresql.yml replace localhost with ip address obtained from previous step for host value in defaults section.

Step 5: Now run yesod devel

sudo docker run --rm -t -i --link postgresql:postgres  --volumes-from postgresql -p 3000:3000 -v /vagrant/yesodprojects/project1:/var/yesodprojects/project1 rpaladugu1/yesod-postgresql /bin/bash -c "cd project1   && yesod devel"
