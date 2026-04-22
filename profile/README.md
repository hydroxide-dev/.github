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

Hydroxide defines resources with two portable files: `compute.yaml` and `service.yaml`. These two work together to deploy an instance.

`compute.yaml` defines *how* to run a VM.

```yaml
version: 1
resources:
  cpu:
    cores: 4
  ram:
    amount: 8192
  gpu:
    type: vgpu # NVIDIA, anyone?
    vram: 1024
  disk:
    - id: boot
      type: boot # Easily define default disk locations
      size: 16384
  network:
    ports: # Native firewalling
		- 80:80/http  # Maybe you have HTTP...
		- 443:8096/https # ... or HTTPS...
		- 25:25/tcp   # ... or SMTP for some reason
    bridges:
      - name: vmbr0
        ipv4: dhcp
        ipv6: slaac
runtime:
  os: hydroxide/image/ubuntu-24-lts
  # or something a little simpler:
  # os: hydroxide/image/debian-13-trixie
  # or something more... rolling?
  # os: spacedouut/hyd-image/endevouros-20260325

```

`service.yaml` defines *why* to run a VM.

```yaml
version: 1
catalog:
  name: jellyfin
  friendly: Jellyfin
  description: Media server
meta:
  author: hydroxide # In relation to the image file, but I digress.
provision:
  packages:
    - jellyfin
  hooks:
    install:
      - systemctl enable jellyfin
    start:
      - systemctl start jellyfin
  health:
    type: http
    action: http://{service}:8096/health
    timeout:
      success: 120s
      failed: 60s
```

Of course, these two are portable in nature. Mix and match them, run whenever.

There's more to Hydroxide than YAML files though. Including but most definitely *not* limited to:
 - IAM based on PAM authentication
 - SSH key login and sync
 - A CLI
 - ... and more to come in the future!

## How do I install?

... You don't. Not yet, atleast! Hydroxide is still in active development. Check back from time to time.
