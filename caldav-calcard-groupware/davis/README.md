# Installing and using the Davis calendar/contacts server

Like [Baikal](../baikal/README.md), Davis is based on the Sabre CalDAV/CardDAV packages, which also means this is a PHP package.  It's also derived from, inspired by, Baikal.

Repository: https://github.com/tchapi/davis

Billed as:

> A simple, fully translatable admin interface and frontend for sabre/dav based on Symfony 7 and Bootstrap 5, initially inspired by Baïkal (see dependencies table below for more detail)

Where this software falls down is configuration and installation.

For example, the authentication is either BasicAuth (which is hyper-not-any-kind-of-best-practice), or using an IMAP or LDAP server for authentication.  Where does one get an IMAP server?  I could probably use the IMAP service from Dreamhost for this.

But the rest of the configuration drove me away trying to read and understand and I simply did not like what was offered for config.

I created this `compose.yml`:

```yaml
services:
  davis:
    # build:
    #   context: ../
    #   dockerfile: ./docker/Dockerfile
    # image: davis:latest
    # If you want to use a prebuilt image from Github
    image: ghcr.io/tchapi/davis:edge
    container_name: davis
    environment:
      - APP_ENV=prod
      - DATABASE_DRIVER=sqlite
      - DATABASE_URL=sqlite:////data/davis-database.db # ⚠️ 4 slashes for an absolute path ⚠️ + no quotes (so Symfony can resolve it)
      - MAILER_DSN=smtp://${MAIL_USERNAME}:${MAIL_PASSWORD}@${MAIL_HOST}:${MAIL_PORT}
      - ADMIN_LOGIN=${ADMIN_LOGIN}
      - ADMIN_PASSWORD=${ADMIN_PASSWORD}
      - AUTH_REALM=${AUTH_REALM}
      - AUTH_METHOD=${AUTH_METHOD}
      - CALDAV_ENABLED=${CALDAV_ENABLED}
      - CARDDAV_ENABLED=${CARDDAV_ENABLED}
      - WEBDAV_ENABLED=${WEBDAV_ENABLED}
      - WEBDAV_TMP_DIR=${WEBDAV_TMP_DIR}
      - WEBDAV_PUBLIC_DIR=${WEBDAV_PUBLIC_DIR}
      - WEBDAV_HOMES_DIR=${WEBDAV_HOMES_DIR}
      - INVITE_FROM_ADDRESS=${INVITE_FROM_ADDRESS}
      - APP_TIMEZONE=${TIMEZONE}
    volumes:
      - /home/david/docker/davis/www:/var/www/davis
      - /home/david/docker/davis/data:/data
    networks:
      - servernet

networks:
  servernet:
    external: true
```

This means you can either plop this into the checked-out Git repository -- or use a prebuilt image from the GitHub image repository.

The environment variables are to come from a `.env` file, and there are a few of those as examples in the repository.

I did not test this any further.  I did not feel good about this project.
