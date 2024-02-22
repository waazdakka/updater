# 🤖 Updater ⬆️

This app makes it possible to update an existing Nextcloud Server installation in an automated or 
semi-automated fashion. When you initiate an update of your Nextcloud Server (either from the command-line 
via `updater.phar` or via the Web UI) this is the app that is triggered.

> [!TIP]
> If your installation is broken, your environment does not meet our system requirements, or you are doing
> something exotic, the automated Updater may not be appropriate. Your use case and circumstances may require
> you to follow the manual update process described in the Maintenance chapter of the [Nextcloud Admin Manual](https://docs.nextcloud.com).

## Background

Fully updating an existing Nextcloud environment, at a high-level, consists of:

- Determing which update paths exists for the in-place Nextcloud Server installation then alerting the admin of their
availability and explaining their requirements and trade-offs
- Confirming and preparing the in-use installation (e.g. detecting any currently enabled apps that are incompatible with the target
Nextcloud version, triggering some repair and pre-update preparation environment changes)
- Updating the Nextcloud Server software safely (e.g. handling client connections appropriately during the update, verifying the
integrity of the software package, checking the requirements of the runtime environment, handling unexpected problems that arise
during any of the update steps, replacing the existing installation files with new ones, utilizing any customizations you made to your
config that are meant to influence the update process, and keeping your data protected)
- Running the database migrations (i.e. adjusting your installation's existing database tables to accomodate changes
required by the new version of Nextcloud Server).
- Updating existing apps

The Updater app focuses on item #1. After the Updater finishes successfully, item #2 is handled either by manually 
running `occ upgrade` from the command-line or allowing the Updater app to trigger it for you. Once the database 
migrations finish, your Nextcloud Server is running the targeted version of the software.

> [!IMPORTANT]
> A complete upgrade of an existing installation also requires running database migrations which this app does handle directly (though
> it does offer to run `occ upgrade` on your behalf when the Server application code has been successfully updated.

## How it works

The Updater

## Useful commands and tips

* `updater.log`
* `updater.phar`
* occ repair command that resets step files/folders safely/cleanly

## Additional Information

### Development History

* Issue #1: https://github.com/nextcloud/updater/issues/1
* Issue #2: https://github.com/nextcloud/updater/issues/2

### Design Goals

- Simple
- Independent of Server itself
- Robust enough handle server timeouts and connectivity losses
- Cleanly handles clients that attempt to connect to Server while an update is in progress
- Verifies the integrity of installation package before deploying
- Sanity checks the environment before attempting an update (e.g. permissions, etc.)
- Usable from both the command-line and the web
- The web facing piece capable appearing "integrated" into the admin's existing Nextcloud Server Web UI experience
- Reliable across different different types of filesystems
- Built-in real-time reporting as well as debug logging to ease different types of troubleshooting scenarios
- Distributable with the standard Server installation package
- Optionally capable of rolling back updating the installation if any step fails (note: does not cover database migrations failures)
- Optionally capable of triggering the database migrations required to finish a full Server upgrade
