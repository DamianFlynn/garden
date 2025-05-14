---
title: "Implementing Comments on a Static Site"
date: "2018-10-30"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
Status: "Published"
Tags:
  [
    "SSG"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "9d10655d-6a03-49ee-a3fa-4fd9a46dd128",
    "created_time": "2024-07-11T14:00:00.000Z",
    "last_edited_time": "2024-07-19T15:23:00.000Z",
    "created_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "last_edited_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "cover": {
      "type": "file",
      "file": {
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/d58d73ce-2aac-432b-9ba0-472b97dd0e9a/banner.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CIMMLBF%2F20250513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250513T105529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIDAhoFeXJzWz8tnlsVTyaIqX7mdJT7y5UGUSGvI%2FwrsMAiEAk%2BbgcbQSw9P0zf%2Bu4FQttnXJAqCHnQ6z1NcA2lW55nQqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF5mjTOkoEMyXZEoPircA0p6jkPeeEOM8lL4QjEPUM4YE%2FV34NAcR5557QExZEu59IVCsXEJicVvCf3mMq0ThmcDY6bNR0tG2J9h0W2Gd9KFq7i%2BTb1qBDuroj9WDlwMKJGPf%2BLNR2aZoXKb6aT3XJjro2xDMMs%2FBVseJx5nRENoXCwpI0kuI%2FYlgNRDKs9bJ0rIq%2By4IFIr%2F4OikTgRp7IYVDfngpQurMLkVoWSzqSSczJsbU1hKKYkJKmTdxIKzrboczutmurHhM77ceeapXZ08Md0v%2BAnkCGWWwQ69KhMfhZ0ypnuJLzTYe8SpDXmO3T6sDhSiQeBGC0DcGkusUTUk9%2F0LB3PgJG28W2l9VQRtBhqbKk23nCobVg5253bI3%2FuYM5bHzUj2ArZAPqx65n2CbEag0gMPt6CjKckiCXo7i3AwkUu56Z9%2BdtWKtcN4CLVdJgZCtTw9FKjA4KNhuiwsCivYaSMYezlTIZzND84bLWnSYj8kdNCjzpMQ41SSBXIuFP4LrLYSGloF6kNTzGg0GTPMDPYroFRIeUjh9%2F4Af28L2sx9prPGcL5CI%2FTszGMGN9aS3lnPAUct0N3hEDfVbclYgNES4CdIyKAwrGskUCQDBbLPVo669kP9JLkX%2BrPZRgsQ%2FUvM5siML7BjMEGOqUBZ%2FFJFf0rhPoCdKoUfDO7%2BaQj3YXstNVqPKImka%2F8fLmpiVSVlSWnAEBj5rAzGAXg%2FLj5NOczeY3m6FdPxeH%2BxyLN3G7eMU978FOrDRVHglIgerfkcK9472j7KwY698bEucZL2q8vtNoraZfLYubNaZKbauMCYBJ8yvOqfX7PUtA6QAP3waFopooMyoBijuUfXLj4%2BmO%2F%2FbEPEF55MtcZVKkRpk3G&X-Amz-Signature=9e0beb50c20b495eea9a2e8e60efcceaab0dcdeee3a88c707e66f4a33f90d363&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-13T11:55:29.453Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Implementing-Comments-on-a-Static-Site-9d10655d6a0349eea3fa4fd9a46dd128",
    "public_url": null
  }
UPDATE_TIME: "2025-05-13T10:57:19.119Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
---

At this point we are almost ready to go live with our site, however, one of the cornerstones to growing and sharing is communication.

# Wordpress

In the world of Wordpress, this was a standard core feature, which leveraged the fact that the pages were rendered on demand from a backend database. In this scenario, the same approach is offered to maintain a commenting platform.

However, as I noted earlier; given that Wordpress powers a very large portion of the blogging surface of the internet; it is an obvious target for hacking, just refer to the CVS database for a glimpse of what this looks like in reality.

I have had more than over of these exceptions result in defacement or excessive spam in the comment system. The real objective, however, is to bloat the database which will then result in the site going offline as the database reaches its maximum limit based on your host or plan.

Recovering from this mess is slow and painful, and you must also not ignore the fact that you now should also update the runtime; a process we try to ignore as this exercise generally results in breaking extensions and taking the site offline for a little time.

# Static Sites

So, if we do not have the luxury of a database to host our comments in the static site configuration (recall all we have is HTML and client-side JavaScript); how on earth do we implement this critical feature.

Of course, we can use the cloud! There are many SaaS offerings which are designed to integrate into our site but offload all the storage and processing to the service

Additionally, many of these are free to use, if you agree to let the service display a couple of advertisements. Previous I have used services from Disqus on my site to offload the challenges of hosting and keeping updated my own.

## Disqus Out!

As I coded the liquid for this side I also implemented Disqus as the commentary service. However, immediately after turning this on for my posts the page load time was almost 3 times slower!

Adding insult to injury the adverts have evolved to be click bate and not relevant to my content what so ever.

Disqus does offer a *Not Free* option which addressed the Advertising a bit better, but that does not explain why the massive performance hit?

Tracing my site loading time with Chromes F12 development tools expose the shocking truth.

> Adding Disqus to the site results in over 50 treads to tracking and other undesirable sites

Therefore I immediately deleted the liquid code and stopped any further integration of this service. It’s gone and good ridden

## Utterance.es

Watching how Microsoft recently replaced their commentary service on the *docs.microsoft.com* sites to leverage GitHub, I decided that this might be a really good solution for this site also.

After a little research, I found a lovely match called **utteranc.es** which requires that you log in with your Github account, and will create a new issue per post in my site, that can be tracked and managed as normal issues within github which is pretty awesome.

(I assume based on my content and audience that this should not be a problem - let me know on Twitter if I am wrong about this)

### Implementing Utteranc.es

Adding this feature is trivial, Really trivial!

1. We require a public Github Repo
1. Authorise the [Utteranc.es Bot][https://github.com/apps/utterances] access to the selected Repo
1. Add the following javascript code to our page, updating the paramater repo="[ENTER REPO HERE]" to match the Repo name; for example repo="[damianflynn/damianflynn.github.io"
```javascript
<script src="https://utteranc.es/client.js"
        repo="[ENTER REPO HERE]"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
```

I have added a little extra logic to determine which pages to offer comments; for example; I do not need this feature on the main landing page.

## Summary

Now, I really want you to tell me what your thoughts about this for a solution?

Go On, Leave a comment, even if its just a thumbs up or down!

