---
layout: post
date: '2017-08-22T11:14:29-04:00'
title: "Building Our Ideas App"


description: By leveraging an existing open source project, we were able to get planninglabs.nyc up and running in a couple of days.  We chose to clone 18f.gsa.gov and modify it because it had everything we needed.  In fact, much of Labs’ mission and philosophy is inspired by 18F's, so it makes sense that their site would have a lot of the features we want to have on ours, specifically beautiful and informative project pages, a project index with cards, a blog, responsive standards-based design, and a great content that helps to explain their principles.

excerpt: By leveraging an existing open source project, we were able to get planninglabs.nyc up and running in a couple of days.  We chose to clone 18f.gsa.gov and modify it because it had everything we needed.  In fact, much of Labs’ mission and philosophy is inspired by 18F's, so it makes sense that their site would have a lot of the features we want to have on ours, specifically beautiful and informative project pages, a project index with cards, a blog, responsive standards-based design, and a great content that helps to explain their principles.
image:
authors:
- mattgardner
tags:
- open source
- backendless

hero: false
---

We needed a way to receive project ideas from our internal customers, so we built an interactive submission, exploration and discussion app at [ideas.planninglabs.nyc](https://ideas.planninglabs.nyc/). We set out to achieve three things:

- Provide a way for our customers to see what kinds of ideas people have submitted.

- Create a more open project pipeline to allow for future projects to be sourced from the ground up.

- Promote our services as a division.

<figure>
  <img src="{{site.baseurl}}/assets/img/blog/building-ideas-app/ideas_app.png" alt="Screenshot of Ideas App" width="915" />
  <figcaption>We built a simple app to allow users to search and filter Idea submissions, and comment on them.</figcaption>
</figure>

Because it is a "project about projects", we didn't want to spend too much time on it, only just enough to get it started without ignoring our design and engineering principles. To achieve this, we used 2 technologies:

*[Create React App](https://github.com/facebookincubator/create-react-app)* - a CLI (command-line interface) developed by Facebook that takes care of much of the development environment boilerplate for a react app. It's opinionated-ness meant we didn't have to make decisions about how the app compiled or which CSS preprocessors we used.

Additionally, we wanted to create an SPA (single page app) because we knew the ability to dynamically filter pieces of information would be important, but we didn't want to devote time to building a complex custom backend. An SPA would allow us to accomplish this, as well as provide a better overall user experience.

*[Airtable](https://airtable.com/)* - a self-proclaimed "spreadsheet as a database", this service provides a familiar Excel-like interface to create data, and exposes that data through a JSON API.  (The API is the important bit here, it basically means airtable can be used as an "instant backend".  It's a database, and a UI for administering that database, and it includes user authentication)

These two decisions combined allowed us to complete the project within a week while delivering a functional and well-designed product.
There were, however, some caveats. Airtable and Create React App are both opinionated solutions that exist to solve an abstract case, or about 90% of our needs. Both required some small modifications and workarounds:

Airtable reduces the friction between receiving data and exposing that data through an API. However, it did not provide the degree of custom authorization that we needed for the submission process to meet our needs.

Basically, we needed a publicly-accessible JSON feed of project idea data, but didn't want to include all columns, or all projects from our table. (Some projects aren't ready to share yet, and some columns aren't relevant to the public listing).  This meant we needed to make authenticated API calls to airtable, so we built a very lightweight [express.js](https://expressjs.com/) backend to proxy the requests from the client to airtable, adding an API key that we can keep secure. ([The code for this express proxy is also open](https://github.com/NYCPlanning/labs-ideas-api), if you want to check it out.)

<figure>
  <img src="{{site.baseurl}}/assets/img/blog/building-ideas-app/diagram.png" alt="Schematic of API Data Flow" width="915" />
  <figcaption>A simple express.js app proxies requests from a public JSON endpoint to Airtable, adding a private API key</figcaption>
</figure>

Create React App did not include a CSS pre-processor as part of its build step as a matter of its own engineering philosophy. The maintainers recommend "that you don’t reuse the same CSS classes across different components" and therefore, "CSS preprocessors are less useful". We needed to re-tool the CLI to involve a preprocessor in its buildstep. Thankfully, this was a breeze due to the power of good documentation.

The frontend app itself contains two views.  The main view includes a card for each projects, and allows for searching and filtering.  The idea view is a dedicated page for each idea, and implements [Disqus](https://disqus.com/), so that anyone can comment on the idea and show their support (or criticism).

In this project we embraced a balance of using existing tools while extending these tools as needed to ensure on-time delivery of a successful product.  We also had a low-risk, low-stress first project that our young team could collaborate on.  We were able to learn each other's strengths and weaknesses, understand each other's development philosophy, learn which tools and workflows are we all prefer, and work together to get something useful shipped.

**Costs:** 15 person-days, nominal hosting costs

Check out [ideas.planninglabs.nyc](https://ideas.planninglabs.nyc), or [see the source code on github](https://github.com/NYCPlanning/labs-ideas).
