#summary Release Notes for 0.5
#labels Phase-Historical,Deprecated

# Introduction #
We're pleased to announce release 0.5 of Ruby PDNS.

This is a feature enhancement release, this release keeps stats on a per record basis - usage count and total time spent serving a record.  Full details on this feature can be found at [ReferenceStatistics\_0\_5](ReferenceStatistics_0_5.md)

This release is available as a tarball, RPM, Source RPM and RubyGem on the [downloads](http://code.google.com/p/ruby-pdns/downloads/list?can=2&q=0.5) page.

This release does not break any backward compatibility with your records, however you do need to make a small change to the configuration file.  Please see further down below.

## Documentation ##
We tag the documentation for each release so you always have a good point in time reference, please find below links to Reference documentation for this branch

  * [Install Guide](ReferenceInstallGuide_0_5.md)
  * [Configuration Reference](ReferenceConfigurationGuide_0_5.md)
  * [Constructing Replies](ReferenceConstructingReplies_0_5.md)
  * [About Requests](ReferenceInfoAboutRequests_0_5.md)
  * [Statistics](ReferenceStatistics.md)
  * [GeoIP](ReferenceGeoIP_0_5.md)
  * [Ruby Extensions](ReferenceRubyExtensions_0_5.md)

## Upgrading ##
Upgrading should be simplest with the RPM, the major change is a few more config options, a cronjob and some extra files.

### Source Installs ###
Upgrade the gem using the normal Gem commands, then do:

  * Create the directory _/var/log/pdns/stats_ and set ownership to _pdns:pdns_

Create the stats cronjob in _/etc/cron.d/rubypdns_:

```
*/5 * * * *  pdns /usr/sbin/pdns-aggregate-stats.rb --config /etc/pdns/pdns-ruby-backend.cfg
```


### RPM Installs ###
The RPM should cleanly upgrade using just _rpm -Uvh ruby-pdns-0.5.el5.noarch.rpm_

### Configuration ###
This release requires you to make changes to your config file, for a full reference of the config file please see [Configuration Reference](ReferenceConfigurationGuide_0_5.md), the config changes in this release are:

  * To keep stats use the _recordstats_ option
  * Statistics gets written to a directory, configure it with the _stats\_dir_ option.

## Changelog ##
  * Add stats per record for usage count and time spent processing records [#2](http://code.google.com/p/ruby-pdns/issues/detail?id=2)