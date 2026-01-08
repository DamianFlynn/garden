---
title: "Multi-tenant Azure dev setup (AZ CLI + VS Code + direnv + Starship)"
date: "2026-01-07T11:17:00.000Z"
lastmod: "2026-01-08T11:15:00.000Z"
draft: true
featuredImage: "./cover-525d0357.jpg"
Author: "Damian"
authors:
  [
    "Damian"
  ]
Tags:
  [
    "Azure",
    "Identity",
    "DevOps",
    "IaC",
    "Bicep"
  ]
Status: "In Progress"
summary: "Unlock the secrets to seamless multi-tenant Azure development with this comprehensive guide on using AZ CLI, VS Code, direnv, and Starship to prevent costly mistakes, streamline your workflow, and ensure your deployments are both safe and efficient across multiple environments."
Categories: "Azure Management"
NOTION_METADATA:
  {
    "object": "page",
    "id": "525d0357-8eea-4c0a-9115-70e1bc690042",
    "created_time": "2026-01-07T11:17:00.000Z",
    "last_edited_time": "2026-01-08T11:15:00.000Z",
    "created_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "last_edited_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "cover": {
      "type": "external",
      "external": {
        "url": "https://storagebucketn8n.blob.core.windows.net/notion/525d0357-8eea-4c0a-9115-70e1bc690042.png"
      }
    },
    "icon": {
      "type": "emoji",
      "emoji": "🛰️"
    },
    "parent": {
      "type": "data_source_id",
      "data_source_id": "0cb08ce3-4a92-421c-8f01-a3d2151ce62e",
      "database_id": "f9f77549-5ec7-48f1-a25a-aaeed54650ab"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Multi-tenant-Azure-dev-setup-AZ-CLI-VS-Code-direnv-Starship-525d03578eea4c0a911570e1bc690042",
    "public_url": null
  }
UPDATE_TIME: "2026-01-08T11:16:23.040Z"
last_edited_time: "2026-01-08T11:15:00.000Z"
---

If you work across multiple Azure tenants, you’ll eventually have a near miss. It usually looks like this: you open VS Code, run a quick `az login`, and five minutes later you realise you’re authenticated to the last tenant you used, with the wrong subscription selected.

The tooling did exactly what it was designed to do. It just didn’t match the way you’re working.

This is the setup I use to make multi-tenant work boring and safe on both Windows and macOS. The goal is not a clever terminal. The goal is to make it hard to deploy to the wrong place by accident, while keeping the flow fast enough that you’ll actually stick with it.

### Why this happens

Azure CLI caches tokens and stores its active context in a single folder.

On macOS and Linux:

```plain text
~/.azure
```

On Windows:

```plain text
%USERPROFILE%\.azure
```

That’s fine when you live in one tenant.

It’s a liability when you’re switching between customer tenants and using separate identities. The CLI will happily reuse whatever token cache and last subscription it has available. That’s how you end up running commands with more confidence than you should.

### The fix that actually works

Azure CLI honours an environment variable called `AZURE_CONFIG_DIR`.

If you set it, the CLI will read and write auth tokens, subscriptions, and context files in that folder instead of the default `~/.azure`.

I keep separate contexts like:

```plain text
~/.azure-contexts/fjordkraft.no/
~/.azure-contexts/elmira/
~/.azure-contexts/innofactor/
```

Once you do this, switching projects can also switch your Azure CLI context. That removes a whole category of mistakes.

### The workflow I’m aiming for

When I enter a repo folder, I want three things to happen automatically.

1. The right tenant context is selected.
1. If I’m not logged in for that tenant, I’m prompted to log in.
1. I’m defaulted into a safe subscription, normally test or sandbox, not production.
That last point is the difference between a nice developer experience and an operationally safe developer experience.

### Start with a folder layout that matches how you think

If your filesystem is a mess, your context switching will be a mess.

A simple, predictable structure works well.

macOS:

```plain text
~/Development/<org>/<repo>
```

Windows:

```plain text
C:\Users\<you>\Development\<org>\<repo>
```

I like to align `<org>` with the GitHub org name so it stays consistent across machines.

### Use direnv to auto-load tenant variables per repo

`direnv` is a small tool that automatically loads environment variables when you `cd` into a folder, and unloads them when you leave.

This is the glue that turns “I must remember to set AZURE_CONFIG_DIR” into “it just happens”.

### Put a .envrc in every tenant repo

This is the whole pattern.

```bash
# ~/projects/tenant-a-app/.envrc

export AZURE_TENANT_NAME="fjordkraft.no"
export AZURE_TENANT_ID="0c45021a-6a20-4da9-8965-640117142210"
export AZURE_SUBSCRIPTION_ID="636126c6-b929-401c-8977-041791d92e0f"

# Keep Azure CLI tokens/context separate per tenant
export AZURE_CONFIG_DIR="$HOME/.azure-contexts/$AZURE_TENANT_NAME"

# Optional visual cue for your prompt
export PROJECT_PROMPT="Elmera"

# Login if not already logged in
if ! az account show > /dev/null 2>&1; then
  echo "Logging into Azure for $AZURE_TENANT_NAME..."
  az login --tenant "$AZURE_TENANT_ID"
fi

# Set the active subscription
az account set --subscription "$AZURE_SUBSCRIPTION_ID"
```

A couple of practical notes.

* Keep the .envrc in the repo so the team shares the same defaults.
* Using the tenant name in the path makes the context readable when you’re debugging.
* Treat AZURE_SUBSCRIPTION_ID as a safety rail. Make it your test subscription.
### The safety pattern: default to test, switch to prod deliberately

If you do one thing from this post, do this:  **Set your **`**.envrc**`** to a non-production subscription**.

When you actually intend to run against prod, make it a conscious act.

```bash
az account set --subscription <prod-subscription>
```

I want to feel that switch. I want it to be slightly annoying. That’s the point.

### Make the terminal tell the truth (Starship)

Once you’ve got context switching working, the next failure mode is simple: you forget what subscription you’re on.

A prompt that includes tenant or subscription context is a cheap guardrail.

Starship is cross-platform, fast, and works well with this approach. Here’s a minimal custom segment that prints the first part of the subscription ID.

```toml
[custom.azure]
command = "echo ${AZURE_SUBSCRIPTION_ID:0:8}…"
when = "test -n \"$AZURE_SUBSCRIPTION_ID\""
style = "italic green"
symbol = "🛰️ "
```

Could you make it nicer? Absolutely. But even this tiny cue is enough to stop you doing something dumb when you’re tired.

### Make sure VS Code inherits the same context

This setup only works if VS Code’s terminal is using the same shell that has `direnv` hooked.

The simplest habit is: open the repo folder directly in VS Code. Then open the integrated terminal.

If `direnv` is working, you’ll see it load the `.envrc` when you enter the folder.

After that, this should be true without you typing usernames or passwords.

```bash
az account show
```

### Add discipline with git and pre-commit

When you’re working across tenants, mistakes are rarely big dramatic failures.

They’re small, boring, avoidable errors.

* Committing something you shouldn’t.
* Inconsistent formatting and linting.
* Secrets ending up in the wrong place.
`pre-commit` is a good baseline to enforce hygiene before your changes ever hit CI.

### Make it repeatable with direnv

If you want this to scale beyond your own laptop, you need reproducibility.

A dev environment definition, such as `direnv`, helps keep the team aligned on expected tool versions, required hooks, and a consistent way to bootstrap a repo.

The benefit is not standardisation for its own sake. The benefit is fewer surprises when somebody new clones the repo and starts working across tenants on day one.

### The operational flow: test, then what-if, then deploy

Once your local context management is solid, the deployment strategy becomes straightforward.

* Deploy to a test subscription first, to prove the code actually works.
* Run what-if against the target subscription.
* Only then do the real deployment.
That three-step flow turns “looks fine in review” into “I have high confidence this will behave”.

