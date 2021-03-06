---
layout: post
title: Configure the AWS CLI
date: 2016-12-26 00:00:00
description: Tutorial on how to configure the AWS CLI for your AWS account.
comments_id: 14
---

To make it easier to work with a lot of the AWS services, we are going to use the [AWS CLI](https://aws.amazon.com/cli/).

### Install the AWS CLI

AWS CLI needs Python 2 version 2.6.5+ or Python 3 version 3.3+ and [Pip](https://pypi.python.org/pypi/pip). Use the following if you need help installing Python or Pip.

- [Installing Python](https://www.python.org/downloads/)
- [Installing Pip](https://pip.pypa.io/en/stable/installing/)

<img class="code-marker" src="{{ site.url }}/assets/s.png" />Now using Pip you can install the AWS CLI (on Linux, macOS, or Unix) by running:

``` bash
$ sudo pip install awscli
```

If you are having some problems installing the AWS CLI or need Windows install instructions, refer to the [complete install instructions](http://docs.aws.amazon.com/cli/latest/userguide/installing.html).

### Add Your Access Key to AWS CLI

We now need to tell the AWS CLI to use your Access Keys from the previous chapter.

It should look something like this:

- Access key ID **AKIAIOSFODNN7EXAMPLE**
- Secret access key **wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY**

<img class="code-marker" src="{{ site.url }}/assets/s.png" />Simply run the following with your Secret Key ID and your Access Key.

``` bash
$ aws configure
```

You can leave the **Default region name** and **Default output format** the way they are.

Next let's get started with setting up our backend.
