# salt_beacon_demo
Demo of Salt Beacon, event stream and twilio beacon

## Setup

This Salt beacon demo assumes a Salt master and minion running on the same
server. However, some minor changes should make this work in other topologies
as well, but are beyond the scope of this demo.

The Salt master and minion and salt api need to be at least 2015.5 or newer.
The CherryPy python module needs to be installed, too.

```
sudo pip install CherryPy==3.6.0
```

## Configs

Everything found below the `file` directory of this repo should be placed in
the corresponding location on your server.

If you want to use the Twilio beacon, then uncomment and modify
`files/etc/salt/minion.d/beacons.conf` with your Twilio credentials.

Once the config files are place in their proper locations restart the
salt-master and salt-minion. Then start up the salt-api daemon.

I often start up the salt-api in a terminal in debug mode, like the command
below, to make sure there aren't any errors.

```
sudo salt-api -l debug
```

To watch events coming across the Salt event bus run the following command in a
terminal:

```
sudo salt-run state.event pretty=True
```

Now the wtmp beacon will send a Salt event every time there is a system login,
the inotify beacon will send an event when `/srv/blah.txt` and any file under
`/srv/testing` are modified, and the Twilio beacon will send a Salt event any
time a new text message shows up in your Twilio account.

Browse to `http://<server ip>:8000` to view a web page that displays the events
sent by the Twilio beacon. Log in using the user: `api_user` with any text for
a password and `auto` in the last field. Using the `auto` external auth module
is unsafe for production use, but is used here for simplicity in setting up
this demo.

Now every time a text message or mms is sent to the Twilio number, they will be
displayed on the web page.
