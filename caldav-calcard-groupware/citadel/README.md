# Installing and using the Citadel groupware/email/calendar/contacts server

Citadel is billed as _Email, collaboration, groupware, and content management - up and running in minutes, on your own hardware or in the cloud._

The user story is that we want a simple easy way to install this thing, and to use it from inside our outside our home network.  But, the Citadel project doesn't have clear documentation on how to proceed -- well, the project does give a procedure that takes over an entire computer.  But, a homelab person wants to use it in a Docker container that's easy to setup.

Home page: https://www.citadel.org/index.html

Docker: https://hub.docker.com/r/citadeldotorg/citadel -- There is a Docker container.  It does not explain how to use it.  It appears to be built by the Citadel project, and appears to be up-to-date with their recent builds.

There is no documentation on using the container.

The documentation includes the [directory layout](https://www.citadel.org/file_layout.html)

The [manual installation instructions](https://www.citadel.org/system_administration_manual.html) describes taking over an entire computer with the software.  Perhaps one could setup a virtual machine (using Lima?) and follow those instructions.

From the above, a Compose file was created: [compose.yml](./compose.yml)

Using that, the following ports existed in the resulting container:

```
d7734a9e51df   citadeldotorg/citadel  "/usr/local/bin/ctdl…"   3 seconds ago   Up 2 seconds \
    25/tcp, \
    80/tcp, \
    110/tcp, \
    119/tcp, \
    143/tcp, \
    443/tcp, \
    465/tcp, \
    504/tcp, \
    563/tcp, \
    587/tcp, \
    993/tcp, \
    995/tcp, \
    5222/tcp 
        
    citadel

```

Some of these are obvious - SMTP (25), HTTP (80), HTTPS (443) and 5222 is probably related to SSH.

What to do when hosted behind a proxy server...?

How many of these need to be exposed to public ...?
