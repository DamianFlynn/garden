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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/d58d73ce-2aac-432b-9ba0-472b97dd0e9a/banner.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZT7DB5G%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T120923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQD0fuWIrCqgak16VjFqzUDaGGPmEd6%2Bi4ug3is%2ByhZipQIgCD38fsERZMn38iecjD26PGlgJnAUmuCUgw8APF%2Fn5asq%2FwMIFRAAGgw2Mzc0MjMxODM4MDUiDFTPkIC1JSu6Rasj6ircAwEx2UfxFUzMeBTIjBHwVXoMG158vEghuXUHJdzBcdR2NCMoeAM3pn9HUKhnZEdFBi936aCFpZKkjTQlv%2BZett9gDoYawifQvGxoix6pkUiE0A5JsFzQABZRz%2FZnj4d86dpcU9p2UaiY%2B0Y3ppZsd2U0mQWTDwGlhMgIZxbpF78D9TlP86pRpYj6sROeVGfmBuIGGZX1aH2MyYnQVo%2BgeSZ6vGdDcydmNATAc%2Fsl7gqxmosUCMfEv7mGopdsyiyLHDVqBxYqrXG%2BtPVJizkob5uWnI3BavB5DPXd6WHOF3z3%2FRdi8H%2BDcoUpyqhB2U%2Fe34xub7kPCdvU8r6eZ1fnb2r2B94A56IYT%2FIMV0n%2B17jnObkksNRK5pk88Hv4YMYS1dhnPltnBBeXRZLPKsrHwf%2BGHSINqbIzQ7gYYYlLBa3s2ILaE0zgzqNzQTGRD%2BYMWPqBoK%2FptrAWWWmldIGgJT0kFVW40LNEx9GShvg8zwiX4r4wg9sdnbK7SMEcdV0GgaeA%2Fgd4%2B75oIMxSjXfIHsnDy56pVc0gmdIbYHQZ%2FYCA52IvydehKAZ6fVujGLeU6hxrz5vIithgAXxU%2Fy4O13%2F7lHqTjbrKcaziG7MpCxFHMfAceUEVA5eip94PMImGksEGOqUBA2GBoYj9u39%2FFYYpNpSzN92Q6eWWbanFnVSperWQe7xuE8GyXpPhKgJgvPxZn8sOw4jaPjIRKZf0mXMN3viNflA3myYzpdHdOIGn3g%2FlvXsMhjQ1zbyzyy0AjU42aBAj2dDDLoaHywWvQ5ov1SI4S3bUZiIi%2BJJ0AqHxz8mkysrShyhZHEh4H4MgEzDwOfxi1o0PPFx5Mmqzx6kmzCRMyghu99t0&X-Amz-Signature=2e6245971203b14fc835946c3a986de0165949d76d7a88cd3f82d0590a3281c8&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T13:09:23.038Z"
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
UPDATE_TIME: "2025-05-14T12:11:49.109Z"
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

