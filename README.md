# smart-proxy
Welcome to the smart-proxy wiki!

This project is proof of concept. We will try to model asynchronous request-response implementation with RabbitMQ and Spray(Akka) microServices.

Here is [Overview](https://github.com/irybakov/smart-proxy/wiki/Overview)

# Quick Start

First of all you should install [Vagrant](https://www.vagrantup.com/)

Then follow steps:
   - Download  Vagrantfile from this repo
   - run "vagrant up"
   - wait for setup to complete
  
# How to verify
   
   POST http://192.168.34.10:8082/api/ping

   body
   {
    "ping": "How are you?"
   }

Should get
   
   {
    "pong": "PONG: How are you?"
   }
