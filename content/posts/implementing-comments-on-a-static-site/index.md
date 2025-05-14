---
title: "Implementing Comments on a Static Site"
date: "2018-10-30"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
featuredImage: "./cover-9d10655d.jpg"
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/d58d73ce-2aac-432b-9ba0-472b97dd0e9a/banner.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXSKFPNB%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T143510Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQDx5XfXUW7Cs54HNZZkEHFpFbZlv24weKEXpFXomO4%2F8AIhAJ1L9WyLf46%2B4agk%2Ff%2B%2BvlDNpJTeI3DgUEOYanjUG6JdKv8DCBcQABoMNjM3NDIzMTgzODA1Igwts1QFaUsqFGq7R5sq3AMJ%2F62AugFt4k5ocFo9dMlQi6Hv0Y2uJXwMCNJ%2BwaeGvwbstlU%2BtFJvpFneH8tjP57a72js2PzDYXoYG9v02g2DbE7hlR8QHcTeQq5rfRcFFSeyHRKdEF9fbFD%2Fr8IELMcvvevXSO5HsWERoZw64nvmyGyUEiI1KYx8i06x%2BV86VQLYNaFlKLFcsW%2BjAXY6ktLt6X7lNVc4nLvEsVAAE5zY2Xik3Ub9uJ2MGQ6ydm8KrDFtbCUEdgVhMjl9JedN1Vukt255BOWfSEVqQ9nVNvE2wzcVFG%2BPqwaSoW0POEspt%2BfT1jMQg5dWDqwOFo742%2BcqnUhHzgwVEgsPdwWqC%2BXy%2Fu4Gtx2qUYtkR5Ka0K58v%2FWv7P%2BblUPJixOeXhOznnR3288HAGgpoWnYV15WplQiI0eQ5sjNMC%2BcywDpvNpLhKbo5GxkIr0ZxUeKNQJULDr9i3zv52iX1QpqVq98ecmKiCjQ4K5qHyssAGDYkYyHlQr7mTTaBx74ArkXqt%2BxJWX7DObei%2BXkPKcTCCGpY94Pylrxet44oz62ycW1ZlkSWksXEIMFxV31TdsC%2FfyNe5B%2FA8ez4DM0oRgWdpfS9EiMHW2P%2Bt3vwHpwR5P7SPVunHFmxoRhUY97HJHrLzD%2FupLBBjqkAUoMNn2Db060301O%2F2Hgh6tlKDn7i5%2BCbqRfTbG7Ouxh%2F3BWJAfqPNPyl%2Bhr4TLDmRPihpYBc2OC6pzStkvROyGPcRjeFylhxqd7KJrWVd3iDo6b9TpLqv1MgDGaZMfwOsI%2FvVjvn%2FI0Dxo6xVpg6yWKLgU7F5R9vLtSy%2Bd9yI2qO0Ws6l2m7OF01EDBDnGWJrwSj1XGrsjk3m5ga9xgd7hX5u0X&X-Amz-Signature=d03c650709c2787bbcf42dbe7c8d563583948e1371cfccde2a396800d569dd72&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T15:35:10.690Z"
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
UPDATE_TIME: "2025-05-14T14:37:21.185Z"
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

