---
title: Creating backups
description: Backups can save you a lot of headache in case something happens to your installation or device.
cms:
  collection: guides
  entry: creating-backups
  edit: true
sidebar: default
prev: /help/guides/source-migration/
next: /help/guides/reading-local-manga/
---

Backups are inter-compatible between **Tachiyomi** versions, with a few exceptions listed under [forks information](#forks-information).

Backups can hold the following information:

1. **Titles**
2. **Categories**
3. **Read chapters**
4. **Tracking settings**
5. **Reading history**

:::guide
You can do this by going to <Navigation item="more"/> → <Navigation item="settings"/> → <Navigation item="settings_backup"/> and then selecting **Create backup** and select a location to save it.
:::

## Restoring

Restoring is done from the <Navigation item="settings_backup"/> settings. To restore without issues be sure to:

* Install all the <Navigation item="tab_extensions"/> that were installed at the time of the backup.
* Log into the <Navigation item="settings_tracking"/> services that you use.
* Log into **MangaDex** using **WebView**[ as explained here](/help/faq/#no-results-when-searching).
* You're able to access all the sources through the **WebView**.
* Be on a consistent internet connection.

## Transferring downloads

You can transfer downloaded manga chapters from one version of **Tachiyomi** to another.

:::guide
You can do this by going to <Navigation item="more"/> → <Navigation item="settings"/> → <Navigation item="settings_downloads"/> and then set the download directory to that of the download folder of your old **Tachiyomi**, then restore the backup.
:::

## Turning on auto-backups

It is highly recommended you turn on auto backups, this will ensure you can recover if need be.

:::guide
You can do this by going to <Navigation item="more"/> → <Navigation item="settings"/> → <Navigation item="settings_backup"/> and then setting a **backup frequency**. In the case of a catastrophic failure, at least you will be able to recover.
:::

## Forks information

<div class="text-2xl"><Navigation item="neko"/></div>

This fork can only restore **MangaDex** entries in a backup. If there are entries from other sources that are a part of that backup then they will not transfer.

<div class="text-2xl"><Navigation item="tachiyomiaz"/>, <Navigation item="tachiyomisy"/>, <Navigation item="tachiyomieh"/></div>

These forks have some built-in extensions, namely: **E-Hentai**, **Hitomi.la**, and **nHentai** for <Navigation item="tachiyomiaz"/> and <Navigation item="tachiyomieh"/> while <Navigation item="tachiyomisy"/> only has **E-Hentai**.

Titles from said extensions will not be restored when restoring a backup made by these forks to standard Tachiyomi. It is possible to restore titles to these forks, but it is not possible to back them up out of these forks.

<div class="text-2xl"><Navigation item="tachiyomij2k"/></div>

This fork can only restore legacy backups, as it has not made the changes needed to support the new backup yet.
