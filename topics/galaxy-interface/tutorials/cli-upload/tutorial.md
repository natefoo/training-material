---
layout: tutorial_hands_on

title: "Uploading data from the command-line"
zenodo_link: ""
questions:
  - How can I upload my data to Galaxy when it's not on the same computer as my web browser?
  - What happens if I'm uploading a very large file and the connection is interrupted?
objectives:
  - Install the galaxy-upload utility
  - Upload data to a Galaxy server from the command line
  - Interrupt and resume an upload
time_estimation: "30M"
key_points:
  - The galaxy-upload utility can be used to upload data from any system with a command line and network access
contributors:
  - natefoo
subtopic: features
tags:
  - data
  - upload
---

The standard method for uploading data to a Galaxy server is via the Galaxy user interface, in your web browser. This presents a challenge when the system your browser is running on is not the same as the one where your data are stored. While it may be possible to first copy your data from where it lives to your computer, this is inefficient and may even be impossible depending on the size of the data and the network speed.

If the system where your data is housed has a method for command-line access and is allowed to make outbound connections to a Galaxy server, then you can cut out the middleman and just upload data directly from your remote system to Galaxy.

> ### {% icon comment %} History of large data uploads
> Prior to the release of Galaxy 22.01, the primary method for uploading large data to Galaxy was via the FTP service. FTP upload support was added to Galaxy at a time when HTTP-based upload methods were unreliable and limited. Over the years since, institutional firewalls have made usage of the FTP service increasingly difficult, while HTTP-based upload methods have improved.
>
> New upload functionality using a robust, modern protocol ([tus.io](https://tus.io/)) was added to Galaxy in version 22.01. In August 2022, the FTP service on usegalaxy.org was decommissioned. Other Galaxy servers may continue to run an FTP upload service, but the method described in this tutorial works with *all* Galaxy servers running version 22.01 and later.
{: .comment}
