# Galaxy - ChemicalToolBox
#
# VERSION       0.1

FROM bgruening/galaxy-stable 

MAINTAINER Pablo Moreno, pablacious@gmail.com

# Install all requirements for PBS 
RUN DEBIAN_FRONTEND=noninteractive apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y torque-client
RUN DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y wget
#RUN DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y cdbs 
#RUN DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y debhelper 
RUN DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y libtorque2-dev 
#RUN DEBIAN_FRONTEND=noninteractive wget ftp://ftp.surfsara.nl/pub/outgoing/pbs_python.tar.gz 
#RUN DEBIAN_FRONTEND=noninteractive tar -xf pbs_python.tar.gz
#RUN DEBIAN_FRONTEND=noninteractive cd pbs_python*
#RUN DEBIAN_FRONTEND=noninteractive debian/rules binary 
#RUN DEBIAN_FRONTEND=noninteractive 
 

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /galaxy-central

RUN DEBIAN_FRONTEND=noninteractive LIBTORQUE_DIR=/usr/lib/ python scripts/scramble.py -e pbs_python
RUN service postgresql start && sh run.sh --daemon && sleep 120


# Compile DRMAA
WORKDIR /
RUN DEBIAN_FRONTEND=noninteractive wget http://downloads.sourceforge.net/project/pbspro-drmaa/pbs-drmaa/1.0/pbs-drmaa-1.0.17.tar.gz 
RUN tar -xf pbs-drmaa-1.0.17.tar.gz
WORKDIR /pbs-drmaa-1.0.17
RUN ["./configure"]
RUN ["make"]
RUN ["make","install"]

WORKDIR /galaxy-central
# Mark one folders as imported from the host.
VOLUME ["/export/"]

# Expose port 80 to the host
EXPOSE 80

CMD ["/usr/bin/startup"]
