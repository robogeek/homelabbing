# Installing and using the Radicale calendar/contacts server

Radicale is a lightweight CalDAV/CardDAV server that implements a subset of the relevant specifications.  It claims to be easy to install.  It is a Python application, and most of the documentation regards installing it on a native host under Apache or whatnot, perhaps using USWGI.  But, there is a Docker container which of course eases the task.

Pay attention to a note at the bottom describing how Radicale doesn't implement everything, and that someone looking for a complete CalDAV/CardDAV solution should look at other servers.  A list of alternatives is given.

Home page: https://radicale.org/

Docker container: https://hub.docker.com/r/grepular/radicale

Repository for container: https://gitlab.com/grepular/docker-radicale

Main repository: https://github.com/Kozea/Radicale

The main website has documentation on the file structure to setup, and the configuration file.  It's a little on the sketchy side (doesn't tell enough).  But there's enough here to work out how to configure a Compose file to launch the container.

Compose file: [compose.yml](./compose.yml)

The file to be mounted on `/etc/radicale/config` is the configuration file.  While reading the documentation, I thought - that's gonna be a directory with possibly multiple config files.  No.. it's a file, not a directory.

This is the config I wrote:

```
[auth]
type = htpasswd
htpasswd_filename = /etc/radicale/users
htpasswd_encryption = autodetect

[server]
hosts = 0.0.0.0:5232, [::]:5232
```

There are many other options, but this worked for me.

The file `/etc/radicale/users` is an htpasswd file.

On Ubuntu that command had to be installed this way:

```
 1191  sudo apt-get install apache2-utils
 1192  htpasswd
```

Then, you initialize the password file as so:

```
 1195  htpasswd -c config/users david 
```

The program asks for a password then generates the file.

I'm using Caddy as the reverse proxy, which means using this configuration:

```
http://dav.example.com {
  redir https://{host}{uri} permanent
}

https://dav.example.com {
        reverse_proxy radicale:5232
}
```

To test the application - on Ubuntu, I created a new Online Account that's a WebDav account.  You find this in GNOME Settings under Online Accounts.  On macOS, in the Settings there is an area for Internet Accounts.  In GNOME the account type is WebDav.  On macOS, there are separate account types for CalDAV and CardDAV.

In GNOME, to create the account specify the domain name for the server (`dav.example.com`), and separately give the user name and password.  In macOS, specify the domain name and user name as an e-mail address: `USER@DOMAIN`.

The next step - creating calendars - was unclear from the documentation.  It discusses creating a directory and adding a properties file in the directory.  But, that was not recognized.

Instead, one simply uses a web browser to visit `https://dav.example.com`.  You'll find a login form.  After logging in, you can create calendars and address books.

Using the GNOME Calendar application, I am able to see the calendar which was created, but am unable to add events to the calendar.  It seems the calendar is read-only.

