# Ruby development framework for Power DNS Pipe Backend #

**NOTE: The source for this project is now on**<a href='http://github.com/ripienaar/ruby-pdns'>github</a>. 

A lot of cases require custom DNS responses based on location, time of day, monitoring status or many other situations, traditional DNS hosting systems makes this **very** hard.  [PowerDNS](http://www.powerdns.com/) makes this a bit easier for the skilled hacker with it's [Pipe Backend](http://doc.powerdns.com/pipebackend-dynamic-resolution.html) but the documentation and implementation details can be quite scary, what if someone made a simple framework to make this easy?  This is that framework.

The simplest way to show what it does is by example, here is a record that does Geo Location based responses for _www.your.net_:

```
module Pdns
  newrecord("www.your.net") do |query, answer|
    case country(query[:remoteip])
      when "US", "CA"
        answer.content "64.xx.xx.245"

      when "ZA", "ZW"
        answer.content "196.xx.xx.10"

      else
        answer.content "78.xx.xx.140"
      end
  end
end
```

Place this file in _/etc/pdns/records/www.your.net.prb_ and it would get served with full Geo capability.  Replace it with a newer version and it will be reloaded and served without any need to restart your pdns server.

The language is Ruby, a number of [language extensions and helper functions](ReferenceRubyExtensions.md) are provided to do common things like [Geo lookups](ReferenceGeoIP.md), randomization and so forth and effort has been made to make it intuitive even for non programmer to write simple records, perhaps by using recipes on this site.  Being that you have the full power of ruby at your hands right in your nameserver, the possibilities is not just GeoDNS but really anything you can imagine.

This framework allows you to do this and much more.  Look at the [Introduction](Introduction.md) page in the wiki for more overview information.

I aim to make this the ideal platform to build Cloud services on, developers and platform managers need fine control over their DNS, traffic and locations of their services.

**Current Release:** 0.5 [fixed issues](http://code.google.com/p/ruby-pdns/issues/list?can=1&q=status=Verified+milestone=Release0.5), [Release Notes](ReleaseNotes_0_5.md)

[Full Changelog](Changelog.md)