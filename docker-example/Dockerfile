FROM mill.jfrog.team:12030/tomcat:latest

MAINTAINER jbaruch@jfrog.com

ADD war/app.war /home/exec/tomcat/webapps/app.war

CMD /bin/bash -c cd /home/exec; /home/exec/tomcat/bin/catalina.sh run
