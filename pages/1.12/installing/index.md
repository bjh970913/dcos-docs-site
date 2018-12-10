---
layout: layout.pug
title: Installing
menuWeight: 30
excerpt: Installing the Enterprise and Open Source versions of DC/OS
---

# Welcome to the DC/OS Installation Page

DC/OS currently supports two main installation methods:

## DC/OS on Cloud Providers with the Mesosphere Universal Installer 

Use the [Mesosphere Universal Installer](/1.12/installing/evaluation/mesosphere-supported-methods/)  to deploy DC/OS on Amazon Web Services (AWS), Azure Resource Manager (AzureRM), and Google Cloud Platform (GCP). The Mesosphere Universal Installer is built on top of Terraform, an open source infrastructure automation tool which uses templates to manage infrastructure for multiple public cloud providers, service providers, and on-premises solutions. Through the Mesosphere Universal Installer you can quickly and easily create your infrastructure, configures resources, and manage communication between agents. Operations such as scaling a cluster or upgrading to a newer version of DC/OS are now incredibly easy. Getting started templates allow for a quick installation of DC/OS, yet can be expanded into a full production environment including upgrades, without ever taking down your cluster.

## Production Installation

The [production installation](/1.12/installing/production/) method is used to install production-ready DC/OS in on-premesis environments. This method was previously called custom installation. It involves packaging the DC/OS distribution and connecting to every node manually to run the DC/OS installation commands. This method is recommended if you want to integrate with an existing system or if you do not have SSH access to your cluster.

