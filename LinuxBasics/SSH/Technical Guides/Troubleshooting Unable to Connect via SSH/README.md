# Troubleshooting SSH: Unable to Connect

### Resources

## Table of Contents
### Scope
### How to Use This Guide
### SSH Requirements
### Common SSH Issues
<p><br>
</p>

## Scope

This guide is to provide guidance on a methodical approach to troubleshooting issues with connecting to a remote server via SSH. The scenarios covered come from my 7 years of Cloud Hosting with 6 of those years as a Linux System Admin. The goal is to verify and hopefully eliminate the most common scenarios that would result in someone being unable to connect to their remote Linux server in an effort to reduce the time it takes to resolve the issue at hand. This guide will not be covering any hosting provider nor any server management application specific scenarios.


## How to Use This Guide

The general flow of this guide is investigate and verify external causes and work towards server-side issues and/or configuration set-ups that most commonly prevent a remote SSH connection. The idea is to troubleshoot what is within the end-users control and work through the network layers until you are looking at server side configuration files and/or logs via a recovery console to investigate possible causes for a failure to connect. Depending on the nature of the error and where the initial failure appears to occur, skip to the section that covers what you haven't tried yet


## SSH Connection Requirements

In order to connect to a Linux server via SSH, there are several common things to consider: 1) Is there a firewall between you and the Linux server that may be rejecting/blocking your connection? 2) Is the SSH service running on the Linux server? 3) Are you connecting to the correct port that SSH is listening on?