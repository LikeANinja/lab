# Homelab Infrastructure

Debian 13 homelab deployed via Ansible.

Includes:

-   Rootful Podman (storage on `/srv`)
-   UFW (LAN + Tailscale aware)
-   Samba
-   Jellyfin
-   Navidrome
-   Audiobookshelf
-   Nextcloud
-   Immich
-   Pi-hole
-   Optional Tailscale

Everything is automated and survives reboot.

------------------------------------------------------------------------

## Requirements

-   Fresh Debian 13 install
-   `sudo` access
-   Git installed

------------------------------------------------------------------------

## Quick Start

### 1) Clone

``` bash
git clone <your-repo-url>
cd lab
```

------------------------------------------------------------------------

### 2) Inventory

`inventory.ini`

``` ini
[homelab]
localhost ansible_connection=local
```

------------------------------------------------------------------------

### 3) Variables

`group_vars/all.yml`

``` yaml
lan_subnet: "192.168.1.0/24"
allow_tailscale: true
podman_bin: "/usr/bin/podman"
```

------------------------------------------------------------------------

### 4) Create Vault File

``` bash
ansible-vault create group_vars/vault.yml
```

Example:

``` yaml
samba_users:
  - name: ninja
    password: "strong-password"

nextcloud_db_password: "db-password"
immich_db_password: "db-password"
```

------------------------------------------------------------------------

### 5) Run

``` bash
sudo ansible-playbook -i inventory.ini site.yml --ask-vault-pass
```

Optional: configure `ansible.cfg` with a vault password file to skip
prompts.

------------------------------------------------------------------------

## Service Ports

  Service          Port
  ---------------- -------
  Jellyfin         8096
  Navidrome        4533
  Audiobookshelf   13378
  Nextcloud        8080
  Immich           2283
  Pi-hole          8081
  Samba            445

Accessible from LAN and Tailscale.

------------------------------------------------------------------------

## Storage Layout

    /srv/
      containers/
      media/
      backups/

Podman storage lives on `/srv` to avoid filling the root disk.

------------------------------------------------------------------------

## Reset Containers

``` bash
sudo podman system reset -f
```

Then rerun playbook.

------------------------------------------------------------------------

## Philosophy

-   Fully reproducible
-   No manual setup
-   No secrets in Git
-   LAN-first architecture
