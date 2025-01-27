# cnspec

**Open source, cloud-native security and policy project**

`cnspec` is a cloud-native solution to assess the security and compliance of your business-critical infrastructure. `cnspec` finds vulnerabilities and misconfigurations on all systems in your infrastructure including: public and private cloud environments, Kubernetes clusters, containers, container registries, servers and endpoints, SaaS products, infrastructure as code, APIs, and more.

`cnspec` is a powerful Policy as Code engine built on [`cnquery`](https://github.com/mondoohq/cnquery), and comes configured with default security policies that run right out of the box. It's both fast and simple to use!

![cnspec scan example](docs/gif/cnspec-scan.gif)


## Installation

Install `cnspec` with our installation script:

```bash
bash -c "$(curl -sSL https://install.mondoo.com/sh/cnspec)"
```

If you prefer a package, find it on [GitHub releases](https://github.com/mondoohq/cnspec/releases).


## Run a scan

Use the `cnspec scan` subcommand to check local and remote targets for misconfigurations and vulnerabilities.

### Local scan

This command evaluates the security of your local machine:

```bash
cnspec scan local
```

### Remote scan targets

You can also specify remote targets to scan. For example:

```bash
# to scan a docker image:
cnspec scan docker image ubuntu:22.04

# scan public ECR registry
aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/r6z5b8t4
cnspec scan docker image public.ecr.aws/r6z5b8t4

# to scan an aws account using the local AWS config
cnspec scan aws

# to scan a kubernetes cluster via your local kubectl config
cnspec scan k8s

# to scan a GitHub repository
export GITHUB_TOKEN=<personal_access_token>
cnspec scan github repo <org/repo> 
```

## Policies

`cnspec` policies are built on the concept of [policy as code](https://mondoo.com/policy-as-code/). `cnspec` comes with default security policies configured for all supported targets. The default policies are available via the [cnspec-policies](https://github.com/mondoohq/cnspec-policies) GitHub repo.

## cnspec interactive shell

`cnspec` also provides an interactive shell to explore assertions. It helps you understand the assertions that policies use, and write your own as well. It’s also a great way to interact with both local and remote targets on the fly.

### Local system shell

```bash
cnspec shell local
 .--. ,-.,-. .--. .---.  .--.  .--.™
'  ..': ,. :`._-.': .; `' '_.''  ..'
`.__.':_;:_;`.__.': ._.'`.__.'`.__.'
   mondoo™        : :
                  :_;
cnspec>
```

The shell provides a `help` command to get help on the resources that power `cnspec`. Running `help` without any arguments lists all of the available resources and their fields. You can also run `help <resource>` to get more information on a specific resource. For example:

```bash
cnquery> help ports
ports:              TCP/IP ports on the system
  list []port:      TCP/IP ports on the system
  listening []port: TCP/IP ports on the system
```

The shell uses auto-complete, which makes it easy to explore. Once inside the shell, you can enter MQL assertions like this:

```coffeescript
> ports.listening.none( port == 23 )
```

To clear the terminal, type `clear`. 

To exit, either hit CTRL + D or type `exit`.

## Scale cnspec across your fleet

The easiest way to scale `cnspec` across your fleet is to have all of your infrastructure pull policies from a central location. A simple approach is to sign up for a free account on Mondoo Platform. The platform is designed for multi-tenancy and provides a secure, private environment that keeps data about your assets in your own account. With the platform, all assets can report on policies and you can define custom exceptions for your fleet.

To use `cnspec` with the Mondoo Platform, run:

```bash
cnspec auth login
```

Once authenticated, you can scan any target:

```bash
cnspec scan <target>
```

`cnspec` returns the results from the scan to `STDOUT` and to the platform.

### Upload policies to your account

With an account on Mondoo Platform, you can upload policies:

```bash
cnspec policy upload mypolicy.mql.yaml
```

## Custom policies

A `cnspec` policy is simply a YAML file that lets you express any security rule or best practice for your fleet.

A few examples can be found in the `examples` folder in this repo. You can run any of these policies via:

```bash
cnspec scan local -f examples/example.mql.yaml
```

If you're interested in writing your own policies or contributing policies back to the `cnspec` community, see our [policy as code guide](https://mondoo.com/docs/tutorials/mondoo/policy-as-code/).

## Where to go from here

There are so many things `cnspec` can do, from testing your entire fleet for vulnerabilities to gathering information and creating reports for auditors. With its custom policies, `cnspec` can scan any component you care about!

Explore our:

- [Policy Bundles](https://github.com/mondoohq/cnspec-policies)
- [Policy as Code](https://mondoo.com/docs/tutorials/mondoo/policy-as-code/)
- [MQL introduction](https://mondoohq.github.io/mql-intro/index.html)
- [MQL resource packs](https://mondoo.com/docs/mql/resources/)
- [cnquery](https://github.com/mondoohq/cnquery), our open source, cloud-native asset inventory tool
- [HashiCorp Packer plugin](https://github.com/mondoohq/packer-plugin-mondoo) to integrate `cnspec` with HashiCorp Packer!

## Community and support

Our goal is to secure all layers of your infrastructure. If you need support, or want to get involved with the development of `cnspec`, join our [community](https://github.com/orgs/mondoohq/discussions) today and let’s grow it together!

## Development

See our [Development Documentation](docs/development.md) for information on building and contributing to cnspec.

## Legal

- **Copyright:** 2018-2022, Mondoo Inc
- **License:** MPLv2
- **Authors:** Christoph Hartmann, Dominik Richter
