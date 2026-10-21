# Setup check: five questions with a pass and a fail

Two products are on the table here: Cloudflare OS, which deploys into a Cloudflare account on Workers and Durable Objects, and Kortix, the open-source AI Operating System whose deployment is a git repo and whose sessions run on cloud computers. Both are open source. Every question below can be answered from a company's own infrastructure, by whoever runs it, without installing anything. Write the answers down. A yes nobody can point at a document for is a no.

| Question | Pass looks like | Fail looks like |
| --- | --- | --- |
| Can you name the machine a session runs on? | A host, its disk, its backups, and a shell you can open | An account name and a product line, nothing below it |
| Does a permission change land as a diff? | A commit with an author, a review, a revert | A settings page, a ticket to a vendor, a screenshot |
| Can you read last month's record yourself? | An export you run, into storage you own | A request to support, a dashboard you can only view |
| Who can widen an agent's reach? | A named role, and a second person to approve the change | Anyone holding an admin seat, unrecorded |
| If the host changed tomorrow, what moves? | A clone, a config, a start command | A migration project and a support contract |

## The five questions, in full

**1. Name the machine.** Cloudflare OS runs each workspace as a Durable Object and each app in a Dynamic Worker facet, and server code cannot reach the internet except through the capabilities you grant. That is a strong boundary. It is not a host you can log into. Kortix gives each session its own isolated machine and branch. Neither answer is wrong. The question is whether the person who gets paged can describe what they are paging about.

**2. Does a permission change land as a diff?** On Kortix the grant is a block in `kortix.yaml`: `connectors: all` or a list, and an omitted grant resolves to none. The change has an author and a revert like any other commit. On Cloudflare OS the change is a Gatekeeper policy, or an introduction of one resource to one agent, held in the deployment you build around Cloudflare's core. Both are reviewable. Only one of them is reviewable in the repository your engineers already read on a Monday.

**3. Read last month's record yourself.** Every Gatekeeper logs the actions it mediates. Cloudflare OS also attaches what an agent observed to the work itself, so a dashboard built from a sensitive table stays tied to that table's access rules. On Kortix every action is recorded, and reading or exporting the audit log depends on the plan. Ask the boring version of the question before rollout: who hands over the record, in which format, on whose storage.

**4. Who can widen an agent's reach?** The failure mode in every system is a person with an admin seat and a deadline. Cloudflare OS answers it at the platform level: an agent and an app start with access to nothing. Each resource then arrives as an explicit introduction. Kortix answers it in the repo: a grant is visible in the file and reviewed like code, and merge is default-deny for agents. The pass condition is the same in both: two names, one to ask and one to approve.

**5. If the host changed tomorrow.** Cloudflare OS was built to run on Cloudflare. Its repository marks deployment to your own server on `workerd` as coming soon, so the supported move today is another Cloudflare account. Kortix runs on Kortix Cloud, in your VPC, or on your own on-prem network, with self-host free, and the repository follows it. Score this one on how much of the company's context travels with the deployment, not on the host list.

## The Kortix side in two commands

Run these in the directory the company already owns:

```bash
# turn this directory into a Kortix project
kortix init

# check it compiles, then bring it live
kortix ship
```

The generated `kortix.yaml` is the file to read before you commit. Grant each agent exactly the connectors, secrets and skills it needs, then set the approval default you want, because gates are off until you set them. To run the same project on your own hardware instead of the managed cloud, `kortix self-host start` installs and runs the full stack.

## How to score it

Count the passes. Four or five means the operating layer belongs in a repo and on hosts your team can name. That is what Kortix is for. Two or three usually means the company wants a platform team it does not have, and Cloudflare OS is a fair answer, particularly for a company already on Cloudflare with a workforce that wants small apps. Zero or one means no system of either kind should be attached to production data yet.

Cloudflare OS facts here come from the [launch post](https://blog.cloudflare.com/cloudflare-os) and the [core repository](https://github.com/cloudflare/cloudflare-os), both read in October 2026. Kortix is open source, so the other side of the checklist can be verified rather than believed: read it, fork it, audit it at [Kortix on GitHub](https://github.com/kortix-ai/suna).
