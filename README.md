# AWS System Lecture

## Ned Lecture Notes

Instead of hosting websites on Heroku, users can host using Amazon Web Services.

Gain access to computers at Amazon to host services in their Cloud database.

Normally Port8888 is unavailable to the outside world, for security purposes. Usually protected by firewalls.

By assigning a static IP, essentially asks Amazon for a static IP address. Paying a small fee is normally required to obtain a static IP address.

## IP Addresses vs Domain Name

Domain name => URL to access website.

IP Address => 0.0.0.0 => 255.255.255.255

Cloud DNS Service looks up the corresponding IP address that matches the Domain Name.

Throws an error if domain name doesn't exist with a corresponding IP address.

LocalHost is a local server that is run on a personal device, not going up to the Cloud DNS service to look for addresses online.

## Communication Cycle

Communication starts at the DNS service, has to communicate with the user, which will then throw an HTTP request to your server. Servers (ie.. webrick, puma) will then communicate with the actual application (Rails, Sinatra) and then finally communicates with the database that is being used fo the app (PostgreSQL & MySQL).
