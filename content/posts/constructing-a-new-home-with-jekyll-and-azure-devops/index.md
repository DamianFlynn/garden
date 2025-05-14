---
title: "Constructing a new Home with Jekyll and Azure DevOps"
date: "2018-10-10"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
Status: "Published"
Tags:
  [
    "SSG",
    "Jekyll",
    "DevOps"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "b0f635c5-ca9a-46ed-8b53-581245f1e796",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/29596ea8-22ca-4ab8-88cb-fa2b36014ccf/banner.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CIMMLBF%2F20250513%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250513T105529Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIDAhoFeXJzWz8tnlsVTyaIqX7mdJT7y5UGUSGvI%2FwrsMAiEAk%2BbgcbQSw9P0zf%2Bu4FQttnXJAqCHnQ6z1NcA2lW55nQqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF5mjTOkoEMyXZEoPircA0p6jkPeeEOM8lL4QjEPUM4YE%2FV34NAcR5557QExZEu59IVCsXEJicVvCf3mMq0ThmcDY6bNR0tG2J9h0W2Gd9KFq7i%2BTb1qBDuroj9WDlwMKJGPf%2BLNR2aZoXKb6aT3XJjro2xDMMs%2FBVseJx5nRENoXCwpI0kuI%2FYlgNRDKs9bJ0rIq%2By4IFIr%2F4OikTgRp7IYVDfngpQurMLkVoWSzqSSczJsbU1hKKYkJKmTdxIKzrboczutmurHhM77ceeapXZ08Md0v%2BAnkCGWWwQ69KhMfhZ0ypnuJLzTYe8SpDXmO3T6sDhSiQeBGC0DcGkusUTUk9%2F0LB3PgJG28W2l9VQRtBhqbKk23nCobVg5253bI3%2FuYM5bHzUj2ArZAPqx65n2CbEag0gMPt6CjKckiCXo7i3AwkUu56Z9%2BdtWKtcN4CLVdJgZCtTw9FKjA4KNhuiwsCivYaSMYezlTIZzND84bLWnSYj8kdNCjzpMQ41SSBXIuFP4LrLYSGloF6kNTzGg0GTPMDPYroFRIeUjh9%2F4Af28L2sx9prPGcL5CI%2FTszGMGN9aS3lnPAUct0N3hEDfVbclYgNES4CdIyKAwrGskUCQDBbLPVo669kP9JLkX%2BrPZRgsQ%2FUvM5siML7BjMEGOqUBZ%2FFJFf0rhPoCdKoUfDO7%2BaQj3YXstNVqPKImka%2F8fLmpiVSVlSWnAEBj5rAzGAXg%2FLj5NOczeY3m6FdPxeH%2BxyLN3G7eMU978FOrDRVHglIgerfkcK9472j7KwY698bEucZL2q8vtNoraZfLYubNaZKbauMCYBJ8yvOqfX7PUtA6QAP3waFopooMyoBijuUfXLj4%2BmO%2F%2FbEPEF55MtcZVKkRpk3G&X-Amz-Signature=cf1216b60daa4f4cc51ba5e393dbf492f7084db51f371207697529090a43e7d6&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-13T11:55:29.454Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Constructing-a-new-Home-with-Jekyll-and-Azure-DevOps-b0f635c5ca9a46ed8b53581245f1e796",
    "public_url": null
  }
UPDATE_TIME: "2025-05-13T10:57:25.776Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
---

One of the unspoken truths behind the lack of posts in recent history was due to a few bugs, which in the end resulted in an experience where from home it appeared that any new content was published and working; but outside this fortress in the real world, there was a large silence echoing.

I really only discovered this issue in May of this year, and was, to say the least, a little agitated with the situation and decided then to change the approach to how I save my notes and share my thoughts.

# Jekyll

After a lot of hours hacking at CSS and JS, neither of which are my strongest points; combined with a whole lot of *liquid* scripting, which is based on the Python Jinja library; I chose to leverage the open source Jekyll project.

This is not to say, that I might not reconsider this again as I am pretty intrigued also with Hugo; but one point is for sure… My days struggling with *Wordpress* are history.

Don’t get me wrong, Wordpress is great, even fantastic, but when it breaks, or its hacked (and boy have I been hacked), or when the comments system becomes a spam target; then its a total nightmare to have to deal with.

I want something that is easy to use, a lot less prone to hacking, and painless to host; so my choice was clear from the start - I was going to use a Static Site Generator

## Building

Leveraging GIT for my version control, I have a simple pipeline which rebuilds a new version of the site each time a new commit is made to the repository. I do like to tweak and have actually no less than two approaches to the effort

```mermaid
graph LR
  A[Blog Repository]
  B[Build Pipeline]
  C[Docker Based Build]
  D[Native Build]
  E[Publish Built Site]
  A -->|Git Push Trigger| B
  B --> C
  B --> D
  C --> E
  D --> E
```

As I spend the majority of my time focused on Microsoft Technology stack, I am leveraging Azure DevOps to run my build process; however, if you prefer other tools, for example, Jenkins, CircleCI, etc then the concepts should be easily transportable, as there is nothing truly complex happening at this point.

### Docker Build Pipeline

This version of the pipeline is my favourite, as I can use the same commands on my workstation to run a local web server to watch in realtime what my edits are going to look like when I finally commit, with 100% confidence that there will be no drift, as I use the exact same container for both roles, development and deployment

The pipeline I am sharing is in YAML format, which we are going to see a whole lot most of over time, and by sharing this you can easily recreate your own build pipeline with nothing more than a good paste!

The build is running on a hosted Ubuntu 1604 instance, but this could be easily replaced with a dedicated build node; however for the amount of time I will use for the building, I should fall well inside the free monthly allocation offered in Azure DevOps; so, for now, this is perfect.

The pipeline has only 3 steps

```mermaid
graph TD
  A[Retrieve the relevant commit from Git Repo]
  B[Run Docker Image to Build Site]
  C[Move Generated HTML Site to Staging Area]
  D[Publish Built Site]
  A --> B
  B --> C
  C --> D
```

The *YAML* representation of the flow is as follows; you can also choose to add the steps in the UX and provide the data below into the relevant fields, as there is a 1:1 relationship between the UX and the YAML Infrastructure as Code

```yaml
resources:
- repo: self
  queue:
    name: Hosted Ubuntu 1604
steps:
- task: Docker@1
  displayName: 'Run an image'
  inputs:
    containerregistrytype: 'Container Registry'
    command: 'Run an image'
    imageName: 'jekyll/builder:latest'
    qualifyImageName: false
    volumes: |
     $(Build.SourcesDirectory):/srv/jekyll
     $(Build.BinariesDirectory):/srv/jekyll/_site
    workingDirectory: '$(Build.SourcesDirectory):/srv/jekyll'
    containerCommand: 'jekyll build --future'
    runInBackground: false
- task: CopyFiles@2
  displayName: 'Copy Files to: $(Build.ArtifactStagingDirectory)'
  inputs:
    SourceFolder: '$(Build.BinariesDirectory)'
    TargetFolder: '$(Build.ArtifactStagingDirectory)'
- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: _site'
  inputs:
    ArtifactName: '_site'
```

### Native Build Pipeline

The Native approach does not offer a whole lot of immediate advantages over the docker version of the pipeline; I honestly created this to prove to myself that I could.

However, after creating this, I do see an advantage. If I should choose to create a dedicated Build Server; I would be able to have the *Ruby bundler* and all the *Jekyll gems* pre-staged on the node; which would remove almost 3 minutes from the build pipeline, as these steps would not need to be repeated every time I executed a new build.

Now, I would have expected the Docker approach to have this as an advantage with the Gems pre-installed in the container, but that’s not the case with the official container I have used in the other pipeline; As a result, both pipelines take 3.5 minutes to prepare, build and publish my site artefacts currently. Clearly, I have a lot of room to make this better.

This pipeline is a little more verbose with 5 steps currently

```mermaid
graph TD
  A[Retrieve the relevant commit from Git Repo]
  B[Run Docker Image to Build Site]
  C[Move Generated HTML Site to Staging Area]
  D[Publish Built Site]
  A --> B
  B --> C
  C --> D
```

The *YAML* representation of the flow is very similar to the previous sample, this time however you are going to really just been looking at some shell commands, which run essentially on any platform we can host ruby on.

```yaml
resources:
- repo: self
  queue:
    name: Hosted Ubuntu 1604
steps:
- task: UseRubyVersion@0
  displayName: 'Use Ruby >= 2.4'
- script: 'gem install bundler'
  displayName: 'Install bundler'
- script: 'bundle install'
  displayName: 'Install Jekyll and Dependencies'
- script: 'bundle exec jekyll build -d $(Build.ArtifactStagingDirectory)'
  displayName: 'Build Jekyll Static Site'
- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: _site'
  inputs:
    ArtifactName: '_site'
```

## Next Steps

With a built site, published; what we really have is a *.ZIP* file which contains all the generated HTML which we can drop onto our web server to publish to the world.

There are many choices on the web hosting platform to use, keep tuned, and I will share with you the solution I have elected to use for this site

