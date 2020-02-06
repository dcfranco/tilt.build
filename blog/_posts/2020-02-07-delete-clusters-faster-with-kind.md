---
slug: delete-clusters-faster-with-kind
date: 2020-02-07
author: nick
layout: blog
title: "Kill Your Kubernetes Clusters Faster with Kind"
subtitle: "Announcing a Big Improvement in Local Development"
tags:
  - tilt
keywords:
  - tilt
  - kind
  - local development
---

At the ClusterAPI meeting in January

Contributors create a Kind cluster and use Tilt for interactive development

Slow, and hard to optimize. But fresh clusters were a huge improvement.

That's why we're excited to announce some big changes to make Kind + Tilt faster than ever

Use our setup script:

TK LINK

The script may look simple. 

A lot of work from a lot of different people in the Kubernetes community went into making this possible.

How does it work?

## What is Kind?

Kind runs a Kubernetes cluster in a Docker container 

Kind is an acronym for Kubernetes IN Docker

Started as a project for testing Kubernetes

Features:

- Fast startup time, faster teardown time

- Works well in continuous integration for testing apps

- Can run multiple versions of Kubernetes, even one you built yourself

Hearing about Kind reminds me of the first time I heard about the Go language. 

Bunch of senior engineers at Google in a corner, trying to redesign C++ to optimize for fast compiler times.

Absolutely changes how you use Go. You start using it for entirely new use-cases.

## What's New?

To make Kind work for local development, you need:

1) Start the cluster
2) Install your base services
3) Update them quickly with your changes

Kind already made #1 fast.

Tilt, with live_update, makes #3 fast.

The missing piece was #2. 

It's taken a long time, with lots of experiments.

kind load tried dumping your image to a tarball and copying it.

felt like re-implementing a slower wheel.

The insight: We already have a fast way to send images around! 

They're called image registries. They cache layer by layer.

But how to use?

- A container runtime config that allows you to use a local image registry

- A Kind API that lets you patch the cluster runtime config.

- A way to start a registry that's reachable from both local and the cluster

- A way to tell other tooling about this registry

The setup script puts these APIs together.

Lets you push images to a local registry so that they'll be available in-cluster.

No auth, good caching.

Now you've got a way to cold-start services in the cluster quickly and start hacking.

## Is Kind Right for Me?

If you're currently doing multi-service development in Docker for Mac, 
and having performance or stability issues,
try out Kind!

Don't be afraid to delete clusters and create new ones.

If you're on Linux, also look into Microk8s. Yay no VM overhead.

We also plan to write up a similar setup script for K3D  using the same approach described here.
If you're interested in K3D, come talk to us!

Have fun being Kind!

