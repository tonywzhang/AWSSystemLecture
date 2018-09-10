# AWS System Lecture

## Ned Ruggeri Lecture Notes

Instead of hosting websites on Heroku, users can host using Amazon Web Services.

Gain access to computers at Amazon to host services in their Cloud database.

Normally, ports (ie: Port8888) are unavailable to the outside world, for security purposes. Usually protected by firewalls.

By assigning a static IP, essentially asks Amazon for a static IP address. Paying a small fee is normally required to obtain a static IP address.

## IP Addresses vs Domain Name

Domain name => URL to access website.

IP Address => 0.0.0.0 => 255.255.255.255

Cloud DNS Service looks up the corresponding IP address that matches the Domain Name.

Throws an error if domain name doesn't exist with a corresponding IP address.

LocalHost is a local server that is run on a personal device, not going up to the Cloud DNS service to look for addresses online.

## Communication Cycle

Communication starts at the DNS service, has to communicate with the user, which will then throw an HTTP request to your server. Servers (ie.. Webrick, Puma) will then communicate with the actual application (Rails, Sinatra) and then finally communicates with the database that is being used fo the app (PostgreSQL & MySQL).

## Issues with Scaling

This machine does not have enough CPU power to respond to a huge amount of users. If the number of users all have O(n+1) queries, the number of users your app can service will likewise decrease. Either way, the CPU only runs so fast and will eventually fail with an increasing amount of users.

## Possible Solutions

Horizontal Scaling - Adding more machines could help with that solution.

Vertical Scaling - Could also simply try to make our own app more efficient or also improve the specs of the machine itself. Adding hardware to the current machine to improve performance could help as well.

Fix issues that deal with efficiency. Make processes more efficient time complexity wise. Eventually one of the above Scaling processes may be necessary. Improving hardware is an easy way to make life easier for your app.

Vertical Scaling is preferred normally over Horizontal Scaling.

## What Makes Applications Slow

Normally fetching, writing, to and from a database causes an app to slow down. Most of the database lives in a computers' RAM.

Writing a variable used to be written into the hard disk. Rotational drones were used on these computers, and they were written onto a specific position on the disk. Similar to rewinding on a cassette tape or on a record disc.

RAM is similar to a electrical circuit, and reading data from the RAM is much faster than looking up data from the hard disk.

### How Does RAM differ from Solid State Drive

SSD is similar to RAM, electrical based circuit that operates without a mechanical component. Will not be destroyed if a mechanical component crashes. Overall, not nearly as quick as RAM. RAM is roughly 10,000 times faster than the disk. SSD is somewhere in the middle.

## How does the database use RAM/Hard Disk

Facebook would be super slow if it was all stored on the Hard Disk, and manually looked up. Hard Disk still needs to be used to persist databases so that the data is not lost. Once the database is initialized, it can grab all of the data from the DB, and read everything and copy it into the RAM. There will subsequently be two copies of the data in the RAM. Therefore, accessing the data will be much faster. Essentially creates a cache for that DB data.

To summarize, the data needs to be stored somewhat in the Disk, but also store a copy in the RAM for faster look up time.

FB probably has millions more GET requests, than actual POST requests, as users are more likely to be reading other people's posts than creating their own.

### What happens when you have a bigger DB than you have RAM?

Buy more RAM. Can cost up to thousands of $, but it will solve many of an app's issues. At some point, you can't add more RAM. The DB doesn't need to move all of the Disk into the RAM. Most popular data can live in RAM, but majority aren't ever accessed, so that un-used data can stay inside the Disk. It isn't optimal, but it is one solution that will help the computer fight being overwhelmed.

Active Set: Batch of data that is used often.

Keeping an app's active set as small as possible is optimal for maintain health.

Sometimes web app's will deny services for users that send an unreasonable number of requests.

## Horizontal Scaling AKA hiring more workers

Having a server that many machines can converse with (PostgreSQL) is important so that many workers can use the same database.

The user will never know about the number of workers in the background. The user only will interact with the LoadBalancer, which becomes the domain name. DNS re-directs them to the LoadBalancer. The LoadBalancer is configured to work with the machines, and picks one when the user sends a request with all of their information that they send over. That corresponding Rails machine will process that request.

LoadBalancer's job is much simpler than if the user was interacting directly with the machines. Response is sent back from the machine to the LoadBalancer, and that same response is then sent back to the user with updated cookies, etc. LoadBalancer serves as an intermediary between the machines that hold all of the code and the user sending the requests.

LoadBalancer serves all users at a time. Instead of receiving requests and dealing with them one at a time, it deals with them in bulk.

Benefits of having multiple machines is security if one goes down. If the LoadBalancer does not get a reply from one machine, it will send the requests to the remaining machines. It will wait for a response from the broken machine, understand that it is not going to get a response back, and subsequently ignores that machine on future requests.

AWS is great because businesses' requirements for machine number is typically constant throughout the United States. If one company fails, Amazon can shift over those same machines to another company. They continually rent out their machines to new companies, or returning customers if a company needs more machines, or has no more need for them.

LoadBalancer will sometimes randomly choose a machine to send the request to. If the LoadBalancer is the SinglePointofFailure, a second LoadBalancer may be used to avoid random bouts of failure. 
