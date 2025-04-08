# Installing and using the Baikal calendar/contacts server using Docker

Baikal is billed as a _lightweight CalDAV+CardDAV server_, that it offers an _extensive web interface with easy management of users, address books and calendars_, and that it is _fast and simple to install and only needs a basic php capable server._

Home page: https://sabre.io/baikal/

It is a part of the Sabre/Dav project, which are PHP implementations of the WebDAV, CalDAV and CardDAV specifications.  These tools are widely used by other PHP projects.

The Baikal [installation instructions](https://sabre.io/baikal/install/) describe its use on an Apache or NGINX service.

In theory one should be able to use the official Apache container to host Baikal.

The Docker-Baikal [installation instructions](https://sabre.io/baikal/docker-install/) discusses use of a special purpose container [ckulka/baikal](https://hub.docker.com/r/ckulka/baikal).

The [Docker-Baikal GitHub repository](https://github.com/ckulka/baikal-docker/tree/master) has example Compose files including one with [volumes mounted on local directories](https://github.com/ckulka/baikal-docker/blob/master/examples/docker-compose.localvolumes.yaml).

From that, a [compose file](./compose.yaml) was created.

I was able to easily login to the Baikal service, set up a user, and set up a calendar.

Then, using the GNOME Calendars application, I tried to connect to that user/calendar, but the application froze.