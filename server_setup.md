# Server Setup


## Install InfluxDB

To download and install InfluxDB, go to [this page](https://portal.influxdata.com/downloads#influxdb) and follow the instructions. For Debian, it will be something like

```
wget https://dl.influxdata.com/influxdb/releases/influxdb_1.4.2_amd64.deb
sudo dpkg -i influxdb_1.4.2_amd64.deb
```

Once InfluxDB is installed, start it:


```
sudo systemctl start influxdb
```

InfluxDB should be started and should run on boot. You can check the status by running


```
sudo systemctl status influxdb
```

To make your data secure, you must create a user and enable authentication. For more details, you can read about authorization [here](https://docs.influxdata.com/influxdb/v1.4/query_language/authentication_and_authorization/#authorization).


First, create an admin user. To do this, bring up the InfluxDB CLI:

```
influx
```

Then create the user using this command:

```
CREATE USER admin WITH PASSWORD '<password>' WITH ALL PRIVILEGES

```

replacing `<password>` with a password. You can also change the name of the user to something other than `admin`. Once you have created this user, enable authentication. Documentation can be found [here](https://docs.influxdata.com/influxdb/v1.4/query_language/authentication_and_authorization/#authentication). You will need to modify `/etc/influxdb/influxdb.conf`.

To put these changes into effect, restart InfluxDB:

```
sudo systemctl restart influxdb
```

Now that authentication is enabled, to access the database you must provide a username and password:


```

influx -username admin -password <password>
```

The admin username and password should be kept safe and only used to create users and databases. If you would like to create another user, then you can follow the instructions found [here](https://docs.influxdata.com/influxdb/v1.4/query_language/authentication_and_authorization/#authorization). For example, you could create a new user by running:

```
CREATE USER <username> WITH PASSWORD '<password>'
```

replacing `<username>` and `<password>` with a username and password. You can then grant the user access to certain databases by running:


```
GRANT [READ,WRITE,ALL] ON <database_name> TO <username>
```

replacing `[READ,WRITE,ALL]` with either `READ`, `WRITE`, or `ALL`.

To create a database, run the following command:


```
CREATE DATABASE <database_name>
```

For more information see [here](https://docs.influxdata.com/influxdb/v1.4/introduction/getting_started/).


## Install Grafana

After installing InfluxDB, you can install Grafana by following the steps on [this page](http://docs.grafana.org/installation/). For Debian, you will run something like this:

```
wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_4.6.3_amd64.deb
sudo apt-get install -y adduser libfontconfig
sudo dpkg -i grafana_4.6.3_amd64.deb
```

and to run Grafana:

```
systemctl daemon-reload
systemctl start grafana-server
```

If you want Grafana to run on boot (a good idea), then run this command:

```
sudo systemctl enable grafana-server.service
```

Once Grafana is started, the web interface is accessible through `http://[host_name_or_ip]:3000`. The default username and password is `admin` and `admin`. Make sure to change the password as soon as possible. This can be done by clicking on the Grafana logo on the top left, hovering over admin, and clicking on "Profile".

To add InfluxDB as a data source, click add data source on the main screen. Give it a name and select the type ("InfluxDB"). Then enter in the URL. If InfluxDB and Grafana is running on the same server, then `http://localhost:8086` (the default) works. Under "InfluxDB Details", enter the database name you created along with the username and password for a user that has read access to that database. Click "Add" and the database will now be added!

For more advance configuration options for Grafana, you can read about that [here](http://docs.grafana.org/installation/configuration/).

Once Grafana is hooked up with InfluxDB, you can start creating new dashboards and graphs.



