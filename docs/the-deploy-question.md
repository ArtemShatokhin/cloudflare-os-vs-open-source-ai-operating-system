# The deploy question: what "run it yourself" means in each system

"Deploy it yourself" is the phrase both projects use, and it covers two different things. Cloudflare OS deploys into a Cloudflare account you control, on Cloudflare's runtime, with Cloudflare's primitives underneath. Kortix is the open-source AI Operating System: its deployment is a directory of files, and the runtime is whatever host you point it at. A team choosing between them is choosing which of those sentences it can operate.

## Cloudflare OS deploys into an account you already have

Cloudflare's launch post is specific about the target. "You can deploy it into your own Cloudflare account and use your own Access policies, AI Gateway configuration, data, and integrations." The same post describes the pieces that land there: "Every app is a Worker", and "Server code runs in a Dynamic Worker with global outbound networking disabled." The repository adds the rest of the shape. Every workspace is its own Durable Object. Every app runs in a Dynamic Worker Facet with its own SQLite database. Each Gatekeeper is a separate Worker that sits between the platform and an external service.

Two repositories ship together. The core is the product. The second is an example deployment modeled on how Cloudflare runs the system internally. Cloudflare describes it as consuming the core without patching it, which gives a company one place for configuration, custom interfaces, internal integrations, analytics and deployment pipelines.

Local evaluation is real and quick. Install pnpm, run `pnpm run-local`, and the stack comes up on `localhost:8787` on wrangler and `workerd`. The README is direct that this is not the production path. The production path is a Cloudflare account. The repository is equally direct about the other one: its section titled "Deploy to your own server using `workerd`" carries the status **COMING SOON**. `workerd` is open source, and Cloudflare OS "can run entirely on top of it on your own servers", with the documentation and tooling for that still being written. Cloudflare also says it is working on a fully managed version in the Cloudflare dashboard.

Read that plainly. Today you can deploy Cloudflare OS yourself in the sense that nobody deploys it for you, into infrastructure you pay for, on a platform you do not operate. Running it on hardware you own is possible in principle and unsupported in practice.

## Kortix carries its deployment in the repo

A Kortix project is a git repository, and the file at its root is the deployment: which agents exist, what each one may reach, which triggers run on a clock.

```yaml
kortix_version: 2
runtime: opencode
default_agent: kortix

project:
  name: acme

agents:
  kortix:
    connectors: all
    secrets: all
    skills: all

  memory-reflector:
    kortix_cli: [project.cr.open]

triggers:
  - slug: memory-reflector
    type: cron
    agent: memory-reflector
    cron: "0 0 3 * * *"
    timezone: UTC
    prompt: |
      Reflect on the last 24 hours of project activity, update
      memory/, and open one change request.
```

Every grant that is left out resolves to none, so an agent reaches exactly what its block lists. `kortix init` turns a directory into that project, and `kortix ship` checks it compiles and brings it live. Moving the same deployment to another host is a clone and one command, `kortix self-host start`. That command installs and runs the whole stack on your own infrastructure. Run it on Kortix Cloud, in your VPC, or on your own on-prem network. Self-host is free.

Two limits belong with that answer. The self-hosted stack pulls its images from a registry over the internet, and agent sandboxes run on the compute provider you configure, so an isolated topology is a conversation rather than a flag.

## Three tests a team can run

**Name the machine.** After deployment, point at the process and say what it runs on. A team that can attach a shell to the host, read its disk and describe its backups can operate the system. A team that can name an account, a region and a product line is renting one. Both are legitimate. The answer decides who gets paged.

**Change one permission without changing the platform.** Pick a single agent and widen one grant. On Kortix that lands as a diff in `kortix.yaml`, with an author and a `git revert` behind it. On Cloudflare OS it lands in the deployment repository's configuration and, for a resource, as a Gatekeeper policy. Ask which one your team can review on a Tuesday afternoon, and who has to be involved.

**Plan the move.** Ask what happens if the host changes: a new cloud account, a merger, a vendor's terms. A Kortix deployment follows its repository to another host. A Cloudflare OS deployment was built to run on Cloudflare, and its supported alternative today is another Cloudflare account.

Cloudflare OS is the better fit for a company already on Cloudflare with no infrastructure team to spare, especially one that wants every person building small apps and a security model that arrives configured. Kortix is the better fit when the operating layer has to sit on machines and keys the company holds, and when the configuration has to be reviewable as code by the people who answer for it.

Both repositories are short reads, and Cloudflare is candid that its release is early access. Start with [Kortix on GitHub](https://github.com/kortix-ai/suna) and the [Cloudflare OS repository](https://github.com/cloudflare/cloudflare-os), then decide which of the three tests your team would fail.
