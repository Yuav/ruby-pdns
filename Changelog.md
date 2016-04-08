#summary Release history
#labels Phase-Implementation

## 0.5 - 2009/10/05 ##
Release focus: Feature Enhancement

Release Candidates: rc1 - 2009/10/03, Release Notes: [ReleaseNotes\_0\_5](ReleaseNotes_0_5.md)

> |[Constructing Replies](ReferenceConstructingReplies_0_5.md)|[Statistics](ReferenceStatistics_0_5.md)|[About Requests](ReferenceInfoAboutRequests_0_5.md)| |[GeoIP](ReferenceGeoIP_0_5.md)|
|:----------------------------------------------------------|:---------------------------------------|:--------------------------------------------------|:|:-----------------------------|
> |[Ruby Extensions](ReferenceRubyExtensions_0_5.md)          |[Configuration Reference](ReferenceConfigurationGuide_0_5.md)| |[Install Guide](ReferenceInstallGuide_0_5.md)      |

  * Add stats per record for usage count and time spent processing records [#2](http://code.google.com/p/ruby-pdns/issues/detail?id=2)
  * Unknown config lines still crash the runner [#27](http://code.google.com/p/ruby-pdns/issues/detail?id=27)

## 0.4 - 2009/08/30 ##
Release focus: Stability and Robustness.

Release Candidates: rc1 - 2009/08/24, Release Notes: [ReleaseNotes\_0\_4](ReleaseNotes_0_4.md)

> |[Constructing Replies](ReferenceConstructingReplies_0_4.md)|[About Requests](ReferenceInfoAboutRequests_0_4.md)|[GeoIP](ReferenceGeoIP_0_4.md)|
|:----------------------------------------------------------|:--------------------------------------------------|:-----------------------------|
> |[Ruby Extensions](ReferenceRubyExtensions_0_4.md)          |[Configuration Reference](ReferenceConfigurationGuide_0_4.md)| |[Install Guide](ReferenceInstallGuide_0_4.md)|

  * Minor maintainability updates [#13](http://code.google.com/p/ruby-pdns/issues/detail?id=13), [#18](http://code.google.com/p/ruby-pdns/issues/detail?id=18)
  * Unparsable config lines should not prevent the framework from starting [#16](http://code.google.com/p/ruby-pdns/issues/detail?id=16)
  * Records that dont compile or raise exceptions should not cause the framework to die [#14](http://code.google.com/p/ruby-pdns/issues/detail?id=14)
  * New script has been added to facilitate testing of records interactively [#15](http://code.google.com/p/ruby-pdns/issues/detail?id=15)
  * Start putting in place a system to do periodic maintenance and stats tasks [#2](http://code.google.com/p/ruby-pdns/issues/detail?id=2)
  * Add cacti monitoring templates to monitor a generic PDNS install [#17](http://code.google.com/p/ruby-pdns/issues/detail?id=17)
  * The GeoIP code ignored the configured path to the geoip data and so blew up pretty badly, code is now more robust in handling errors and uses the config.  Thanks Christian Keil for the bug report.  [#20](http://code.google.com/p/ruby-pdns/issues/detail?id=20)
  * Added a rubygem build task and updated install docs etc [#19](http://code.google.com/p/ruby-pdns/issues/detail?id=19)
  * Change the logging output to only show the filename not the full path of the file that produced the log entry
  * Now prints the correct file and line where log lines are being created from  [#22](http://code.google.com/p/ruby-pdns/issues/detail?id=22)
  * Modules like GeoIP can now have seperate config stanzas  [#7](http://code.google.com/p/ruby-pdns/issues/detail?id=7)
  * GeoIP now has it's own config bits in config file  [#23](http://code.google.com/p/ruby-pdns/issues/detail?id=23)
  * Initial set of unit tests have been added using ruby Test::Unit  [#24](http://code.google.com/p/ruby-pdns/issues/detail?id=24)


## 0.3.1 - 2009/08/15 ##
Release focus: Bug fixes and improved error handling

  * The GeoIP code ignored the configured path to the geoip data and so blew up pretty badly, code is now more robust in handling errors and uses the config.  Thanks Christian Keil for the bug report.  [#20](http://code.google.com/p/ruby-pdns/issues/detail?id=20)
  * The main loop now has some exception handling and will log a backtrace when it dies, this should help with debugging.  Thanks Christian Keil for the bug report.  [#21](http://code.google.com/p/ruby-pdns/issues/detail?id=21)

## 0.3 - 2009/08/03 ##
Release focus: Get major features completed.

> |[Constructing Replies](ReferenceConstructingReplies_0_3.md)|[About Requests](ReferenceInfoAboutRequests_0_3.md)|[GeoIP](ReferenceGeoIP_0_3.md)|
|:----------------------------------------------------------|:--------------------------------------------------|:-----------------------------|
> |[Ruby Extensions](ReferenceRubyExtensions_0_3.md)          |[Configuration Reference](ReferenceConfigurationGuide_0_3.md)| |[Install Guide](ReferenceInstallGuide_0_3.md)|
Initial public release.