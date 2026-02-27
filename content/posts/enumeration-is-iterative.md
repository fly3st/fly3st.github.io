+++
date = '2026-02-27T16:04:08+02:00'
draft = false 
title = 'Enumeration Is Iterative'
tags = ["enumeration", "nmap", "cpts"]
+++

# Enumeration Is Iterative

When I first started studying penetration testing, I thought enumeration was basically:

```txt
run nmap -> read output -> exploit something
```

I found out quickly that this mindset is broken from the get go, since enumeration isn't a single scan, it's a loop.

## My Initial Mistake

My early workflow looked something like this:

```bash
nmap -sC -sV -p- <target-ip>
```

I would just look at the open ports, identify the service and immediatly google an exploit, which I've heard is something that happen to many aspiring PT's early on.
The problem is that real systems don't just reveal everything in one pass.

## What Enumeration Actually Is

Enumeration is more like:

```txt
extract infromation -> form a hypothesis -> test it -> extract even more information -> repeat
```

Every single piece of data changes the entire attack surface.
An example of this would be:

* Port 80 open
* Apache running
* Visiting site reveals login pannel
* Dirbusting reveals `/admin`
* `/admin` returns a 403
* That suggests access control
* That suggests authentication logic
* That suggests potential weak creds or logic flaws

Each step creates th next question.

## Iteration in Practice

A real session looks more like this:

1. Run a broad scan
2. Notice SMB open
3. Enumerate SMB shares
4. Find readable share
5. Extract files
6. Discover internal hostnames
7. Re-run scan against internal references

Information feeds the next action, it isn't about finding a vulnerability it's more about building an understanding of the system we're trying to exploit.
That shift forced my thinking into reading banners more carefully and inspecting responses, HTTP headers and error messages.
The more I treat it as a feedback loop the more I progress.

## Final Thought

Enumeration is about going as much in-depth as we need to understand how the system works and model an attack surface.
Creating depth only happens we allow ourselves to understand that we don't know enough, so we let every answer create a new question.
