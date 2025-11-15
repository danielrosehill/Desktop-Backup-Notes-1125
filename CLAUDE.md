# Desktop Backup System

## Project Purpose

This repository contains the design and implementation of a backup system for Daniel's Ubuntu desktop that treats the OS as ephemeral while protecting system configuration and state.

## Core Philosophy

**Data Strategy**: Important data lives in the cloud. The desktop is a workspace for repositories, videos, photos, documents, and browsing. The goal is NOT to back up bulk data files, but to preserve the system configuration that makes the environment personalized and functional.

**What Needs Protection**: Configuration files, package lists, custom utilities, and system state - the lightweight files that represent cumulative system customization over time.

## Backup Approach

### Infrastructure-as-Code Methodology

Rather than traditional file-based backups, this system version-controls the full system posture:

1. **Package inventory**: Complete list of installed packages (apt, snap, flatpak, npm, pipx, from-source builds)
2. **System configurations**: Keymappings, desktop environment settings, application configs
3. **Custom utilities**: User-built tools and scripts
4. **Environment state**: Dotfiles, application preferences, project structures

### Implementation Strategy

Use Ansible or similar infrastructure-as-code tools to:
- Capture current system state
- Version control the configuration
- Enable automated recreation of environment on fresh hardware

### Automation Requirements

- **Frequency**: Daily or weekly automated backups
- **Storage**: Both local and cloud backup destinations
- **Maintenance**: Regular updates as system evolves

## Current Protection Layer

**BTRFS Snapshots**: Already in place for rollback scenarios (daily, weekly, monthly). These handle driver issues, update problems, and minor system recovery. Proven reliable.

**Gap**: Snapshots don't protect against hardware failure - hence this project.

## Recovery Scenario

### The Problem

Hardware failure (computer catches fire, motherboard dies, etc.) currently requires:
- Fresh OS installation
- Manual recall of all installed packages and sources
- Reinstallation of custom utilities
- Recreation of configurations
- Hope that cloud sync worked for various tools
- Best part of a week to get back to functional state

### The Goal

After hardware failure:
1. Purchase replacement hardware with similar specs
2. Install base OS (Ubuntu with KDE Plasma)
3. Run automated reprovisioning script or playbook
4. System recreates 80%+ of previous environment automatically

### Acceptable Success Rate

**80% automation** is the target. Perfect recreation isn't required - just enough to avoid the week-long manual setup process.

## Two-Layer Backup Posture

1. **Layer 1**: BTRFS snapshots for minor rollbacks and system hiccups
2. **Layer 2**: Infrastructure-as-code backup for full disaster recovery

## Technical Context

- **OS**: Ubuntu 25.10 with KDE Plasma on Wayland
- **Hardware**: Intel i7-12700F, AMD Radeon RX 7700 XT, 64GB RAM
- **Current Backup**: BTRFS snapshots (working well)
- **Previous Attempts**:
  - Clonezilla (too manual, infrequent)
  - Traditional incremental backups (less useful than BTRFS snapshots)

## Project Deliverables

1. System state capture mechanism (Ansible playbook or similar)
2. Automated backup routine (local + cloud)
3. Recovery procedure documentation
4. Provisioning script for fresh OS installation
5. Regular maintenance workflow to keep backups current

## Key Packages to Track

Examples of installation sources that need tracking:
- APT packages
- Snap packages
- Flatpak packages
- npm global packages
- pipx packages
- Programs built from source (e.g., Claude Code from Anthropic)
- KDE Store additions
- Custom GitHub projects
- System configurations and dotfiles (YADM)
