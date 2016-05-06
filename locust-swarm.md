# A Locust Swarm Can Be a Good Thing!

Steve Jackson (@stevejxsn)

## Goals

* How to start?
* Why Locust?
* How to deal with problems in the architecture?
* Make an argument for starting load testing early
* Tell a compelling Story

## Timeline

* .realtor start - Feb 2014
* Load Test start - 15 Sep 2014
* Soft Launch - 20 Oct 2014
* Launch - 23 Oct 2014

## Baby Steps

Started by looking at the architecture and the user funnel and stubbed out dependencies by using Sinatra.

## Picking a Test Tool

* Siege
* Bees with machine Guns
* Gattling.io
* Locust

## Why Locust?

* Could interact with Rails CSRF token
* Could execute end-to-end user interaction with sessions and cookies
* Expands to multiple slaves to increase load capacity
* Ability to run different cases based on the usage percentages

## Some Locust

Locusts can be set to run within a min and max time, a host and define a behavior. The behavior is just a class with methods which can also be weighted to run according to the weight given to them.

Locust comes with a web UI that shows the end points and stats.

## Methodology

* Start small
* Grow the infrastructure as needed
* See where things go

## Easy Wins

* Added indices
* Faked out data
* YSlow to optimize requests

## Performance Optimizations

* Nginx optimizations
* Rails/Unicorn Optimizations
* Ubuntu Optimizations

## What did I learn?

* Don't wait so long to start load testing
* The conversations drive new requirements
* This stuff is hard to figure out under pressure
* Didn't have confidence to make changes in architecture

## PGBouncer

Stands in front of Postgres and keeps a constant amount of connections open between it and Postgres and manages all of the incoming client connections.

## Game time

* Pre-warm the Elastic Load Balancer
* On-demand limits
* Starting new instances doesn't always work

## Soft launch

* Validates assumptions
* Quick fixes
* Put in some straight up hacks

## Load Testing

* Script is not reality
* Load testing is expensive
* Analysis is expensive
* Definitely worth it

## Some questions

* When should we start?
* How do I convince others to start earlier?
* How to make this cheaper?
