    /***************************************************************************************/

    88888888ba  88888888888        db        88888888ba,   88b           d88 88888888888
    88      "8b 88                d88b       88      `"8b  888b         d888 88
    88      ,8P 88               d8'`8b      88        `8b 88`8b       d8'88 88
    88aaaaaa8P' 88aaaaa         d8'  `8b     88         88 88 `8b     d8' 88 88aaaaa
    88""""88'   88"""""        d8YaaaaY8b    88         88 88  `8b   d8'  88 88"""""
    88    `8b   88            d8""""""""8b   88         8P 88   `8b d8'   88 88
    88     `8b  88           d8'        `8b  88      .a8P  88    `888'    88 88
    88      `8b 88888888888 d8'          `8b 88888888Y"'   88     `8'     88 88888888888

GENERAL INFORMATION
============
This project allows you to instantiate a container, running the Newrelic server monitoring service:

 https://newrelic.com/server-monitoring

With this service you will be able to track various metrics about your server, such as CPU and memory consumption.
The monitoring will also track metrics about the docker daemon, and each running container.

There is a known issue with the latest versions of docker, for which NewRelic has produced a workaround. The issue is explained in this post:

 https://t.co/HAlvkB9bHa

Although there are tons of containers on docker hub with nrsysmond, I did not find any solution which would work with docker >= 1.10, and this is the reason why I created this image.

AUTHORS
=======
This work was originally created and maintained by doublebyte: doublebyte@doublebyte.net

INSTALL
==========
REQUIREMENTS & INSTALLATION IN 3 QUICK STEPS
--------------------------------------------

**Warning**

For using this image you will need version >= 1.10 of the docker engine. If you have an **earlier version**, the location of the docker metrics will be different and the docker monitoring **will not work**.
If this is your case, you will be better off using NewRelic's official version:

~~~bash
 docker pull newrelic/nrsysmond_
~~~

If you have a version of docker that is >= 1.10, you may follow the installation steps.

**STEP 1**

The first thing you need to do, is to get an account at NewRelic (if you do not have one already), and retrieve your license key.
You can do so, by following this url:

 https://newrelic.com/signup

**STEP 2**

You can build this image, by typing from the root of this project:

~~~bash
_docker build -t newrelic_sysmond .
~~~

**STEP 3**

The next and final step is to run a container with this image.

The container will need privileged access to the docker daemon, and it will need to bind to some host directories, in order to track what is happening (I guess this is expected from a monitor). 
Therefore you can run the container, with:

~~~bash
 docker run -d \
 --privileged=true --name nrsysmond \
 --pid=host \
 --net=host \
 -v /sys:/sys \
 -v /dev:/dev \
 --env="NRSYSMOND_license_key=REPLACE_BY_NEWRELIC_KEY" \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v /var/log:/var/log:rw \
 newrelic_sysmond
~~~

The string _REPLACE_BY_NEWRELIC_KEY_ should be replaced by your newrelic key.

After this you can start enjoying your server and docker metrics, in the NewRelic control panel. Just wait a few minutes and go to the server's tab. A server with your hostname, will become visible.

