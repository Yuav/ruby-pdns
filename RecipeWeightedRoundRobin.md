## Weighted Round Robin based Responses ##

It's is often the case that in a Round Robin DNS setup one machine gets too many hits, or perhaps one machine is faster than others or have more bandwidth.  Most resolvers are supposed to honor the returned order of results, but this is not always the case - especially those from Microsoft that has a bug ( [1](http://support.microsoft.com/kb/968920), [2](http://drplokta.livejournal.com/109267.html), [3](http://blogs.technet.com/networking/archive/2009/04/17/dns-round-robin-and-destination-ip-address-selection.aspx) ), causing most traffic to hit certain IPs.

This is very hard to fix or work around as the problem is client side, we can assist things a bit though by fiddling what IPs we return when:

  1. Assuming you have 5 ips
    1. 1.x.x.x - this one gets most of the traffic, we want it to get less and the others more
    1. 2.x.x.x - this one has lots of bandwidth from the ISP we want it to get most traffic
    1. 3.x.x.x, 4.x.x.x, 5.x.x.x - they are equivalent and we just want them to get equal traffic

Thanks to the windows features, if we return the first ip in the answer it **will** get the traffic, simple as that, so the solution is to not return it that often.

```
module Pdns
    newrecord("static1.my.com") do |query, answer|

        ips = ["1.x.x.x", "2.x.x.x", "3.x.x.x", "4.x.x.x", "5.x.x.x"]

        ips = ips.randomize([1,5,3,3,3])

        answer.shuffle false
        answer.ttl 300
        answer.content ips[0]
        answer.content ips[1]
        answer.content ips[2]
    end
end
```

In this sample we create an array of our 5 ip addresses, we then randomize them based on the weights supplied to the _randomize_ method.  This randomizes the results but with a bias based on the provided weights.

We turn off shuffling of the output so that we have good control over the order of priorities and we set custom TTLs on the answers.

Lookups against _static1.my.com_ will now return 3 of the 5 records and the first one will be in the list least often which will help in evening things out a bit, unfortunately DNS is not the best load balancing algorithm but with careful adjusting of the weights you can shift traffic around sufficiently on a site with enough clients.

Here's a graph showing the effectiveness of this technique, this is for email servers and with only 2 in a pool - a setup I did not think would work well with this approach!

![http://www.devco.net/images/weighted-balance-graph.png](http://www.devco.net/images/weighted-balance-graph.png)

In this example I am managing the traffic between the Red and the Green machine, the Green machine is both farthest away to my clients and lowest spec yet it was getting the biggest volume of traffic due to the MS bug.  This was negatively impacting my clients as the server load was always 2 to 3 causing mail delivery delays.

The change was rolled out midnight on the 6th, you can see the bias change from Green to Red over the next hours and stay that way for the days to come.  Since the change there have been zero alerts for load on the servers and no customer complaints, mail queues are down and everyone's happy - result.

In this case I am balancing the traffic 3:1 using the technique above favoring the Red, and it is working 100% spot on.

Another example of balancing traffic - this time web traffic - can be seen [here](http://www.devco.net/archives/2009/08/17/managing_web_traffic_with_ruby-pdns.php).