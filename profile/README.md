# So what's hydroxide?

Hydroxide, according to Oxford Languages, is `a compound of a metal with the hydroxide ion OH− (as in many alkalis) or the group —OH.`. It consists of, as the name suggests, a combination of oxygen and hydrogen. Not to be confused with H2O, which contains two hydrogen atoms.

...but assuming you didn't mean the chemical, Hydroxide is a management layer for Proxmox Virtual Environment.

## A deeper dive

Hydroxide is a project with a simple goal in mind: bring cloud-like hosting and service management to a homelab near you. 

Historically, Proxmox VE has been for power users; people who don’t mind manually managing a virtual machine like it's a physical computer. The kind who:
- need to see the BIOS and manage secure boot keys
- are willing to configure things like PCI passthrough by hand
- and want granular control over *their* services.

However, for the same reason people who praise Linux so much stick to simpler distributions, or avoid it entirely, Hydroxide aims to keep that granular control while handling the tedious parts for you.

## How does it actually work?

Hydroxide configures “services” using YAML. These services are stored in a catalog, where they can then be installed. Once installed, Hydroxide reads the YAML like a cooking recipe:
 - Asks for service-specific options: "Do you want GPU passthrough? Where's your media drive?"
 - Creates the VM (or LXC container!) on your behalf
 - Allocates resources
 - Performs initial setup

End goal: you can view your service’s health and access it at any time from Hydroxide.

It also keeps services in sync, running automations to handle things like:
 - Ensuring your uploaded SSH keys work across all services
 - Keeping Docker images, system packages, etc. up to date
 - Making sure certain system tools (eg. your neovim config) exists across services
 - And much more!

## How do I install?

... You don't. Not yet, atleast! Hydroxide is still in active development. Check back from time to time.
