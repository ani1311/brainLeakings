+++
title = 'kubernetes-0'
date = 2024-04-10T00:45:31-07:00
draft = true
+++

There are a LOT of kubernetes tutorials but the point of this blog is to break it down on the basic level on the why someone would use it. Also, this is from the perspective of a noob, so the hypotheticals in this article about the before kubernetes might be pretty stupid, but as someone who entered tech after it was overtaken by kubernetes, this is what the world before kubernetes feels like to me.

## Where it started?

Let's start with, say you a service and a bunch of computers. Your service is pretty cool, and one computer is not enough to serve it, so you decide to use 7 of those computers you have to serve it. How you decide to do that is you set those computer's on your local network install docker(Read about docker if you don't know what it is, the intuition is pretty simple on why) and run the docker image of your service on all of them. On one of them you decide to run a [reverse-proxy](https://youtu.be/SqqrOspasag?si=G79xzA4PKiTskth5) and this reverse-proxy then distributes a request to the other computers. All good I guess? But I can think of a few problems with this:

* How do you actually run the docker image on all these computers? Do you always manually get a terminal on them and docker run it? You probably would write a script that can SSH into each of them and do a docker run.

* What happens when you want to release the next version of your app? This can have a LOT of problems, do you just run the script again with the new docker image and update all computers at once? What if your new update is bad, and computers start crashing, then you have to run the script again with the lower version.

* What happens with failures? if a computer crashes? That means you need someone to monitor all these computers, and if one of them dies you bring another one up. You can maybe write a script that has a ping on your service with a infinite for loop and a sleep.

* When a computer fails, you have to also update your reverse-proxy to point at the new IP.

* What happens if the computer that runs the reverse proxy itself fails? Now this is a big problem, because now your service has downtime. You need to run and setup a new reverse-proxy ASAP.

## In comes kubernetes

Now say you are very annoyed by these issues, and you decide yo give kubernetes a shot. So you somehow setup a kubernetes cluster with those computers. Let's see how kubernetes solves this:

* In kubernetes world, you call these computers as nodes.

* On these nodes, you can run docker containers. A container running on a node is called a pod (Generally there are more than one containers running in a pod but it's ok for now).

*
