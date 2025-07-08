---
title: 5 minute tasks help save my team's backlog
slug: 5-minute-tasks
description: >-
  When creating and tracking a ticket for the backlog takes more time than the
  actual task
tags:
  - management
  - work
  - thoughts
added: 2025-07-08T03:12:37.928Z
---

I reserve an hour of my workday for a series of 5 minute tasks - production changes that take less than 5 minutes to get out the door. These are never pull requests or releases of our API, mobile app, or web app - rather changes in our low-code Retool or CustomerIO deployments.

These 5 minute tasks means I'm not creating a ticket, collecting feedback, prioritizing, and managing it's place in our development cycle. It also means the team asking for the change gets that change the same day they ask for it.

#### Retool

Buildforce is an operation and support heavy organization. Everyone in the organization uses our Retool deployment nearly 8 hours a day, 5 days a week. 

Retool is a constant source of frustration for me - my development team avoids it, it can be finicky when making changes, and I'm often running up against the limits of what a low-code UI can do - but all that said, it's my favorite tool in our stack. It empowers me to make significant changes in less than 5 minutes. I would never go back to building and maintaining a from-scratch interface, and no CMS tool comes close to what I can do.

Retool sits on top of our GraphQL API, which is a major source of it's power - with GraphQL the client can craft the response payload it wants, so no one has to go update a REST API to include extra data.

#### CustomerIO

CustomerIO is another source of frustration, and similar to Retool - my development team avoids it, it can be finicky when making changes, and I'm often running up against the limits of what the workflow builder can do.

However, also like Retool, it lets me make production changes to our communications engine, which is the life-blood of our labor marketplace. I can change an SMS to a Push notification, add a follow-up Push notification, and monitor open and click-thru rates in real time. 
