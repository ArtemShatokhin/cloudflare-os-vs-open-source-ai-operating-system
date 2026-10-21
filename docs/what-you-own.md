# What you own after the install

An operating system for a company is five things: how it is configured, what its agents know, what it remembers, which keys it can use, and who signs off. Where those five live settles the ownership question. Cloudflare OS and Kortix answer it in opposite directions, and each holds up better on some of the five than others.

## Configuration

A Kortix project is a git repository, and `kortix.yaml` at its root is the operating setup: the agents, the grants, the connectors, the triggers. Configuration sits beside the code the company already reviews, under the same permissions and the same history. Undoing a bad change is a revert.

Cloudflare OS keeps configuration in an example deployment repository that wraps the core without patching it. Cloudflare's launch post describes that repository as the place for configuration, custom interfaces, internal integrations, analytics and deployment pipelines. Your setup is a repository you control. The platform's behavior is a repository you read.

## Skills and memory

Cloudflare OS collects terminology, procedures and the best-known way of doing recurring work into a shared library of context and skills. That is a good idea, and one reason rollouts stick. On Kortix, skills are files in the project, and memory is markdown in the same repo. A remembered fact about a customer has an author and a date. A wrong one gets corrected the way a wrong sentence does, anywhere else in the repository.

What separates the two is whether the accumulated context moves with the company. Cloudflare's curated context lives in the deployment you build around Cloudflare's core. Kortix's lives in the repository the platform reads.

## Credentials

Kortix encrypts secrets at rest with a key per project, and connector credentials are brokered server-side and never enter the machine. A session receives a secret only when the person who started it and the agent's grant both allow it. A runtime environment value stays readable inside that session for as long as it lives, so rotate a value that may have been read.

Cloudflare OS goes further here, and it is worth saying plainly. A Gatekeeper is a Worker in front of one external service that holds the credential, handles OAuth, and hands the agent a small typed API. One Gatekeeper can be scoped to a single repository instead of a GitHub account, let an agent read issues but not source code, and make a merge wait for approval. The credential never reaches the agent or the code it writes. For a company whose worst case is an over-broad API key pasted into a chat window, this is the strongest design in either system.

## Review

Kortix settles who may do what on which resource, with per-resource permissions for people and agents, roles, groups, and an audit trail. Session work reaches `main` through a change request, and merge is default-deny for agents. An agent gets the merge capability only when an admin grants it. Widening that grant is itself a reviewed change. Work that lands has a diff, a branch and a person in front of it.

Cloudflare OS reviews at a different point in the flow. A Gatekeeper logs every action an agent or app performs through it. Anything with an externally visible side effect queues for approval. Most human-in-the-loop setups stop an agent dead until someone clicks. A Gatekeeper simulates the outcome locally instead, lets the agent keep working, and hands the person a batch to approve later. That answers the real failure mode better, which is a person turning on auto-approve because waiting is unbearable.

## Where the difference actually bites

Cloudflare OS also tracks what an agent has observed. Say an agent reads a sensitive table and builds a dashboard from it. Opening that dashboard is then checked against the reader's access to the table. A sensitive read can also block later outbound writes, new collaborators, or a handoff to another agent. The observation log attaches to the work and travels with it, and neither system lets you bolt that on afterwards.

The differences show up in ordinary moments. A security review asks who can widen an agent's reach. On Kortix the answer sits in `kortix.yaml`. On Cloudflare OS it is a Gatekeeper policy. A new hire asks for a different model. On Kortix that is a provider and a key the company holds. On Cloudflare OS it is a setting in AI Gateway. Someone asks whether the company could run the whole thing somewhere else, and the answer starts with where the context lives.

Direction is the last difference. The Cloudflare OS repository says that at this time Cloudflare is not seeking outside contribution, arguing that code is now the easy part and review the hard one. That is reasonable for a project this young, and it means the roadmap is Cloudflare's. A Kortix deployment can be changed in the repository, or forked and changed underneath, which is what open source is for.

Neither arrangement is a trick. Cloudflare built the security model most companies cannot build for themselves, and asks you to run it in a Cloudflare account. Kortix asks you to run the whole thing and hands you the responsibility with it. [Kortix on GitHub](https://github.com/kortix-ai/suna) is where to read the second one before deciding.
