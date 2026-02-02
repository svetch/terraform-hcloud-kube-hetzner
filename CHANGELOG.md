# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### ⚠️ Upgrade Notes

#### NAT Router Users (created before v2.19.0)

If you created a NAT router **before v2.19.0** (when the hcloud provider used the now-deprecated `datacenter` attribute), you may see Terraform wanting to recreate your NAT router primary IPs. This would result in new IP addresses.

**To check if you're affected**, run `terraform plan` and look for changes to:
- `hcloud_primary_ip.nat_router_primary_ipv4`
- `hcloud_primary_ip.nat_router_primary_ipv6`

**If Terraform shows replacement**, you have two options:

1. **Allow the recreation** (simplest, but IPs will change):
   ```bash
   terraform apply
   ```

2. **Migrate state manually** (preserves IPs):
   ```bash
   # Remove old state entries
   terraform state rm 'module.kube-hetzner.hcloud_primary_ip.nat_router_primary_ipv4[0]'
   terraform state rm 'module.kube-hetzner.hcloud_primary_ip.nat_router_primary_ipv6[0]'

   # Import with current IPs (get IDs from Hetzner Cloud Console)
   terraform import 'module.kube-hetzner.hcloud_primary_ip.nat_router_primary_ipv4[0]' <ipv4-id>
   terraform import 'module.kube-hetzner.hcloud_primary_ip.nat_router_primary_ipv6[0]' <ipv6-id>

   terraform apply
   ```

**Note**: Most users (those without NAT router, or who created it recently) are unaffected.

### 🚀 New Features

- **Robot server integration** - Manage Hetzner Robot dedicated servers via Terraform (#1916)
- **Audit logging support** - Enable Kubernetes audit logs with configurable policy (#1825)
- **Control plane endpoint variable** - Customize the k3s control plane endpoint (#1911)
- **NAT-router control plane access** - Access control plane through NAT router when LB has no public IP (#2015)
- **Custom subnet IP ranges** - Define custom IP ranges for control plane and agent subnets (#1903)
- **Smaller network support** - Networks smaller than /16 now supported (#1971)
- **K3s v1.35 support** - Added support for k3s v1.35 channel (#2029)
- **Autoscaler configuration options** - Configurable replicas and resources for cluster autoscaler (#2025)
- **Autoscaler swap/zram support** - Configure swap and zram sizes for autoscaler nodepools (#2008)
- **Packer kernel type** - Configurable kernel type in packer builds (#2009)
- **Packer sysctl config** - Custom sysctl configuration file variable (#2010)

### 🐛 Bug Fixes

- **SELinux policy YAML parsing** - Fixed cloud-init SCHEMA_ERROR caused by improper YAML formatting of SELinux policy file
- **Missing SELinux rules** - Added rules for JuiceFS (sock_file write) and SigNoz (blk_file getattr)
- **NAT router deprecation warning** - Removed deprecated `datacenter` from lifecycle ignore_changes
- **Traefik v34 config** - Fixed compatibility with Traefik chart v34 (#2028)
- **NAT router datacenter** - Fixed datacenter attribute deprecation (#2021)
- **kured_version null** - Fixed null handling for kured version (#2032)

### 🔧 Changes

- **Default k3s version** - Bumped from v1.31 to v1.33 (#2030)
- **SELinux policy extraction** - Moved SELinux policy to dedicated template file for maintainability
- **terraform_data migration** - Migrated from null_resource to terraform_data (#1548)
- **remote-exec refactor** - Improved provisioner compatibility with Terraform Stacks (#1893)

---

## [2.18.6] - 2026-02-01

### 🐛 Bug Fixes

- Fixed Traefik v34 chart configuration compatibility (#2028)
- Fixed NAT router datacenter attribute deprecation warning (#2021)
- Fixed kured_version null value handling (#2032)

---

## [2.18.5] - 2026-01-15

_See GitHub releases for earlier versions._
