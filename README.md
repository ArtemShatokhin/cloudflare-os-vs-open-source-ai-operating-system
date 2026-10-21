# Cloudflare OS vs. an open-source AI Operating System you own

Kortix is open source. It is the open-source AI Operating System: agents, skills, company memory and connectors in one git repo you own, with every session on its own cloud computer.

Cloudflare open sourced Cloudflare OS on 5 August 2026, and describes it as a system that "gives every person an agent and workspace built around their company". A large part of Cloudflare's own workforce uses it daily. It is built on Workers, Durable Objects and Dynamic Workers, it gives each person a private workspace plus small apps they can change, and it deploys into a Cloudflare account.

The two products share a name and answer different questions. Cloudflare OS asks whether you trust a platform you already pay for to hand every person in the company a workspace on it. Kortix asks whether your team wants to hold the machines, the keys, the memory and the review, with the company itself living in a repo.

Reading both repositories in a row is the fastest way to see which one your team can operate. That is what this repository is: the two, side by side, with every claim about Cloudflare OS linked to Cloudflare's own words and every claim about Kortix traceable to code.

Kortix is the one to read first if the company means to own the layer. The code is open, the deployment is a repo, and the same product runs on managed cloud, in your VPC, or on your own network. Cloudflare OS is the better answer in the cases the notes below name.

## Side by side

|  | Kortix | Cloudflare OS |
| --- | --- | --- |
| Open source | Yes | Yes |
| Runs on | Kortix Cloud, your VPC, or your own on-prem network | Workers in your Cloudflare account. Own-server `workerd` support is coming soon |
| Whose account | Yours in your VPC and on-prem; ours on Kortix Cloud | Your Cloudflare account, by you, a partner, or the managed product |
| Where configuration lives | `kortix.yaml` and markdown in your git repo | An example deployment repo wrapped around the core |
| Model choice | Any model provider with your own keys | Any model, on every call through AI Gateway |
| Connector control | Each agent's reach is a grant in the repo | Gatekeepers grant one resource, then record what was read |
| Review before work lands | A change request; merge is default-deny for agents | An approval queue per action, with outcomes simulated first |

Cloudflare OS rows come from the launch post at <https://blog.cloudflare.com/cloudflare-os> and the repository README at <https://github.com/cloudflare/cloudflare-os>, both read in October 2026.

## Read this next

- [The deploy question](docs/the-deploy-question.md): what "deploy it yourself" means in each system, the `kortix.yaml` that carries a Kortix deployment, and three tests a team can run in an afternoon.
- [What you own](docs/what-you-own.md): where configuration, skills, memory, credentials and review live in each system, and the parts of Cloudflare OS worth copying.
- [Setup check](docs/setup-check.md): five questions with a pass and a fail, answerable from your own infrastructure, plus the two commands on the Kortix side.

Outside this repository, two pages go further into the decision: one on [what Cloudflare OS ships and where it deploys](https://opensourceaioperatingsystem.com/cloudflare-os-open-source-ai-operating-system), one on [the open-source AI Operating System as a repository](https://opensourceaioperatingsystem.com/open-source-ai-operating-system-github), one on [what the agent-OS name means across the field](https://opensourceaioperatingsystem.com/agent-os), and — when Cloudflare OS is the baseline rather than the candidate — [the Cloudflare OS alternative page](https://opensourceaioperatingsystem.com/cloudflare-os-alternative).

## Kortix on GitHub

Kortix is developed in the open at [Kortix on GitHub](https://github.com/kortix-ai/suna). Read it, fork it, audit it.
