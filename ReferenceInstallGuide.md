# Installing Ruby PDNS #
## Overview ##
As it's pretty early days for the code base it's only been used on CentOS 5.3 using the Epel RPMs for PowerDNS.

I don't doubt that what I've built will work perfectly fine on other operating systems and will try to update the documentation for other Operating Systems as we get more widespread use.


## Deployment Scenarios ##
There are many ways to deploy this and it will depend on your abilities, requirements and setup.

  * You can forward requests from your main bind setup to a Pdns server on another machine or another port.  If you want to use GeoIP based features you cannot do this as the remote IP will be hidden from you.
  * You can setup a separate IP on your nameservers and set PDNS to bind to that IP, give it a hostname and add NS records pointing at those.
  * You can install separate PDNS servers on your network, give them unique IPs and just point lets say _static.foo.net IN NS <your pdns server here>_ to those PDNS machines, they will be serving up just that record then.
  * If you're an existing PDNS user, you can set up the pipe backend in addition to whatever backends you use now and all should be fine.
  * You could migrate all your DNS to PowerDNS

As you can see there is a ton of options for making this work and the best one totally depends on your needs.

We encourage you to join the [ruby-pdns-users mailing list](http://groups.google.com/group/ruby-pdns-users) to discuss the best scenarios and we might improve docs to show some of these in detail.  You can also [contact the author](mailto:rip@devco.net) to provide consultation services to install or host this for you.

## Requirements ##
  * A recent [PowerDNS](http://www.powerdns.com/) server with the Pipe backend, we test against 2.9.22 at time of writing
  * Ruby, tested against 1.8.5 as provided by CentOS 5.3
  * [Maxmind's Ruby GeoIP](http://www.maxmind.com/app/ruby) plugin
  * GeoIP and GeoIP Data, CentOS provide RPMs in their Extras repository for this
  * If you're going to install using Ruby Gems you need them installed first

## Install PowerDNS ##
I am not going to cover the install of PowerDNS in detail, you should get PowerDNS and PowerDNS Pipe Backend installed, on the CentOS machines I used the packages from [EPEL](https://fedoraproject.org/wiki/EPEL) for this.

Refer to the [PowerDNS documentation](http://doc.powerdns.com/) for full install details, most distributions of Linux should provide pdns packages already too.

## Install Ruby PDNS ##
In the current release - 0.3 as of time of writing - we provide a RedHat Source RPM, noarch RPM and a .tgz file with the code, the RPMs will install themselves just fine, for the tar ball there's a manual process, I'll cover both.

### Installing using RPM ###
  * Get the RPM from the [download list](http://code.google.com/p/ruby-pdns/downloads/list)
  * Install once you've met all the listed requirements above simply install the rpm using _rpm -ivh ruby-pdns-0.3rc1-46.el5.noarch.rpm_
  * The RPM will set up log directories in _/var/log/pdns_ and install all the needed libraries into the Ruby site directory etc, it will also install a config file and sample records.

### Installing using Ruby Gems ###
You can install using the rubygem provided, if we do not provide packages for your distribution this is the prefered method.

  * Get the gem from [download list](http://code.google.com/p/ruby-pdns/downloads/list)
  * Install once you've met all the listed requirements above simply install the gem using _gem install --bindir /usr/sbin install ruby-pdns-0.4.1.gem_
  * Unfortunately as a packaging system RubyGems is just barely functional so you need to do some hand work:
    * Create the directory _/var/log/pdns_ and set ownership to _pdns:pdns_
    * Create the directory _/var/log/pdns/stats_ and set ownership to _pdns:pdns_
    * Create the directory _/etc/pdns/records_

Create the stats cronjob in _/etc/cron.d/rubypdns_:

```
*/5 * * * *  pdns /usr/sbin/pdns-aggregate-stats.rb --config /etc/pdns/pdns-ruby-backend.cfg
```

**Note:** The configuration instructions below assume you have the default config file in _/etc/pdns_ if you install with the gem samples will be in your rubygems directories you'll need to either start a new config file on your own or grab them from there, see _gem contents ruby-pdns_ for the locations.

### Configuring Ruby PDNS ###
You now need to tune the _/etc/pdns/pdns-ruby-backend.cfg_ to your tastes, the defaults given should be fairly obvious and would work out the box on a CentOS machine.  See the full [Ruby PDNS Configuration Guide](ReferenceConfigurationGuide.md) for details on all the possible settings.

### Integrating with PowerDNS ###
In the simplest case where you use PowerDNS just to serve records answered by this system you'd only adjust these settings in _/etc/pdns/pdns.conf_

```
launch=pipe
pipe-command=/usr/sbin/pdns-pipe-runner.rb
pipebackend-abi-version=2
```

There are a whole list of other settings though to tune, please see both our guide on [PowerDNS settings that we've found useful](PDNSConfigurationGuide.md) and the [official docs](http://docs.powerdns.org/).

### Testing ###
The RPM or Gem process above will have installed _/etc/pdns/records/`*`-sample_ on your machine, these records will not be active till you name them correctly, so rename _/etc/pdns/records/foo.my.net.prb-sample_ to _/etc/pdns/records/foo.my.net.prb_

Now start your pdns server and look at the logs, you should see lines like:

```
Aug 10 17:31:05 dev2 pdns[5310]: Backend launched with banner: OK#011Ruby PDNS backend starting
Aug 10 17:31:05 dev2 pdns[5310]: Backend launched with banner: OK#011Ruby PDNS backend starting
Aug 10 17:31:05 dev2 pdns[5310]: Backend launched with banner: OK#011Ruby PDNS backend starting
```

and in your _/var/log/pdns/pipe-backend.log_ you should see many lines like these:

```
W, [2009-08-10T17:31:05.773388 #5320]  WARN -- : 5320 runner.rb:16:in `initialize': Runner starting
W, [2009-08-10T17:31:05.773607 #5320]  WARN -- : 5320 runner.rb:33:in `load_records': Loading new record from /etc/pdns/records/foo.my.net.prb
I, [2009-08-10T17:31:05.773923 #5320]  INFO -- : 5320 runner.rb:129:in `handshake': Ruby PDNS backend starting with PID 5320
```

Now do a simple dig test:

```
% dig +short any foo.my.net @yourns1
dns-admin.unconfigured.ruby-pdns.server.net. ns1.unconfigured.ruby-pdns.server.net. 1 1800 3600 604800 3600
10.0.0.1
```

If you see this then you know the logic in the custom record is being served and you can now continue to writing your own records and hosting them on your server.

### Statistics ###
Detailed statistics about records queries and time spent serving those records get recorded, see [ReferenceStatistics](ReferenceStatistics.md) for details on this.