# u: docker
# p: docker
# groups: sudo

# base has node, npm, git

#######################################
# securitybookers
# build image: docker build -t securitybookers .
# run container: docker run -d -p 3000-3005:3000-3005 -p 5432:5432 -v /Users/dfberry/repos/securitybookers:/home/docker securitybookers
# connect to container: docker exec -it CONTAINERNUMBER /bin/bash
# once on container:
#   start postgresql: sudo service postgresql start
#   run setup script: bash setup.sh


#Debian GNU/Linux 8 \n \l
FROM node:latest  

RUN apt-get update && \
    apt-get install -y  sudo && \
    apt-get install -y nano 

#######################################
# USER config

RUN useradd -m docker && echo "docker:docker" | chpasswd && adduser docker sudo 


WORKDIR /home/docker
#
#######################################


#######################################
# WEB config
EXPOSE 3000-3005
RUN npm install http-server -g && \
    npm install gulp -g && \
    npm istall jshint -g
#
#######################################


#######################################
# DATABASE config
RUN apt-get install -y python-software-properties software-properties-common postgresql postgresql-client postgresql-contrib
USER postgres

#start postgres on boot
RUN    /etc/init.d/postgresql start &&\
    psql --command "CREATE USER docker WITH SUPERUSER PASSWORD 'docker';" &&\
    createdb -O docker docker

# Adjust PostgreSQL configuration so that remote connections to the
# database are possible.
RUN echo "host all  all    0.0.0.0/0  md5" >> /etc/postgresql/9.4/main/pg_hba.conf

# And add ``listen_addresses`` to ``/etc/postgresql/9.4/main/postgresql.conf``
RUN echo "listen_addresses='*'" >> /etc/postgresql/9.4/main/postgresql.conf
EXPOSE 5432
VOLUME  ["/etc/postgresql", "/var/log/postgresql", "/var/lib/postgresql"]

#
#######################################

USER docker
CMD tail -f /dev/null

