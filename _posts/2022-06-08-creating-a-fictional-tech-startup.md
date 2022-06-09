---
title: Creating a fictional tech startup
meta: "Our future plans for creating useful examples, tutorials and solve realistic tech problems"
description: "Our future plans for creating useful examples, tutorials and solve realistic tech problems"
categories: Geovation
slug: "2022-06-09-creating-a-fictional-tech-startup"
author: "Dave Rowe"
tags: []
---

{% include image.html url="/assets/images/creating-a-fictional-tech-startup/product-roadmap.jpg" description="A person pointing to a whiteboard displaying a product development roadmap" caption='<span>Photo by <a href="https://unsplash.com/@slidebean?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Slidebean</a> on <a href="https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a></span>' %}

In the tech team we want to showcase property and geo data using industry-leading tech and programming tools. This could be data we know well, like Ordnance Survey or Land Registry data and APIs, but also any dataset that looks interesting and useful to our members. This may include creating code samples, or tutorials to provide example uses of these sources.

That often feels isolated from the real world. Code samples rarely translate into solving problems that a startup may face. It also won't necessarily relate to the technology that a startup would choose. How can we make these things relatable and useful to our community members?

## A fictional startup

To tackle this we want a theme to link our examples and give them some context. That theme will be a fictional startup within the process of creating a software product. We need to consider the examples as being something that a genuine startup would do, where the integration of a technology or dataset would directly enhance their product.

That allows us to immediately adopt some principles:

- **Choose technologies and tools wisely**. It's not worth being saddled with legacy technology when starting out. But at the same time we don't want to try new tech for the sake of it. Small businesses need to weigh up implementing the latest technology with ensuring that they are adopting 'tried and tested' solutions. It's also important to adopt technologies that are reasonably popular so that people can be found to maintain and develop with them.
- **Avoid cutting corners**. Tutorials will often skip inconvenient problems. If an API requires you to not reveal your API key within the web browser then we can't write a tutorial that does this while saying 'make sure you don't do this in a real app'. What we create should be posible to use in a production app.
- **Demonstrate best practice**. It's not good enough to just provide something that works but doesn't have any longevity. We can't promise that our examples will be free from bugs, or mistakes, but we're also not just creating throwaway code that simply demonstrates an idea.

## Initial idea

We're developing a full-stack example mapping application, called **MapUp**, which will demonstrate the elements that go into creating a software application on the web, on mobile devices, and as a desktop application. It will have features to provide insight into buildings and building usage across different geographies in Britain (statistical, govermental, social, and economic). Like many startups, we will be looking at creating a MVP (minimum viable product) that provides enough features to feedback and valdate the initial idea.

In reality this will never be *quite* like a startup. The end-user of the product is really the Geovation community, rather than imaginary users. But by following the principles we've adopted we should be able to demonstrate a lot of key features.

- A Flutter app. Still a relatively new technology, Flutter has [recently surpassed React Native](https://www.statista.com/statistics/869224/worldwide-software-developer-working-hours/) as the most commonly adopted cross platform mobile app technology.
- A PWA (progressive web app) using ReactJS. This is still one of the most widely adopted frameworks that provides one of the quickest routes to publishing your application on the web, using [component libraries such as Material UI](https://mui.com/).
- Backend services using Flask and Python. Within the geospatial community we still see a large number of people using Python. Flask is [a lightweight framework for creating backend microservices](https://flask.palletsprojects.com/en/2.1.x/) using Python.
- Cloud computing hosting and services. AWS [leads in the cloud market](https://www.statista.com/chart/18819/worldwide-market-share-of-leading-cloud-infrastructure-service-providers/) but this market-share is gradualy reducing. We will continue looking across cloud providers.
- A mix of self-hosted and commercial map services. In many cases startups will need the quickest solution available, which may mean implementing API and tile services from commercial cloud providers. In other situations it can save on costs to self-host solutions based on openly licensed data. We will showcase both options.

What this blog for more information.

## What do you need?

More useful than any of this speculation will be hearing from our own community. Are you thinking about using Flutter and would like some information about it? Is there an API or dataset that you think you may want to use but aren't sure about doing so? We'd love to cover areas that are most useful to our members, and to get feedback from you.

And as ever, contact Lewis or Me (Dave) on Slack to arrange a tech clinic if you have any queries or want a chat!
