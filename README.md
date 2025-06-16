# Advanced SOC Analyst Home-Lab

[![Elastic Stack version](https://img.shields.io/badge/Elastic%20Stack-9.0.2-00bfb3?style=flat&logo=elastic-stack)](https://www.elastic.co/blog/category/releases)

Run the latest version of the [Elastic stack][elk-stack]

Based on the [official Docker images][elastic-docker] from Elastic:

* [Elasticsearch](https://github.com/elastic/elasticsearch/tree/main/distribution/docker)
* [Logstash](https://github.com/elastic/logstash/tree/main/docker)
* [Kibana](https://github.com/elastic/kibana/tree/main/src/dev/build/tasks/os_packages/docker_generator)


> [!IMPORTANT]
> [Platinum][subscriptions] features are enabled by default for a [trial][license-mngmt] duration of **30 days**. After
> this evaluation period, you will retain access to all the free features included in the Open Basic license seamlessly,
> without manual intervention required, and without losing any data. Refer to the [How to disable paid
> features](#how-to-disable-paid-features) section to opt out of this behaviour.

---



## Overview

Building an **advanced SOC Analyst home lab** by deploying a honeypot in an Azure virtual machine, intentionally exposing it to the public internet to attract **real-world attacks**. Set up log *forwarding to centralize logs and failed intrusion attempts*, then integrate this repository with a SIEM. Finally, use the SIEM to query and visualize the data, **creating an attack map** that displays the *geographic origins of the attacks*.


---

## Contents

1. [Requirements](#requirements)
   * [Host setup](#host-setup)

1. [Let's Get Started](#let's-get-started)
   * [Create Resource Group](#create-resource-group)
   * [Create Virtual Network](#virtual-network)
   * [Create Virtual Machine](#virtual-machine)
   * [Start Kibana](#start-kibana)
   * [Start Logstash](#start-logstash)
   * [Add Elk Stack into Windows Environmental Variables](#add-elasticsearch-logstash-and-kibana-into-windows-environmental-variables)




















## Requirements

### Host setup

* [Install Elasticsearch](https://github.com/elastic/elasticsearch/tree/main/distribution/docker)
* [Install Logstash](https://github.com/elastic/logstash/tree/main/docker)
* [Install Kibana](https://github.com/elastic/kibana/tree/main/src/dev/build/tasks/os_packages/docker_generator)

> [!NOTE]
> Any version of Elasticsearch below 7.0 (specifically Elasticsearch 6.x and earlier) requires you to manually install the JDK (Java Development Kit)
> because it does not come bundled with Java.

### Software Prerequisites
* **PowerShell / Command Prompt:**
     * For running `.bat` scripts and managing services.
* **JDK (Optional)**
     * Elasticsearch comes with a bundled JDK; no need to install unless custom configuration requires it.
* **7-Zip or WinRAR**
     * To extract `.zip` distributions of Elasticsearch, Kibana, and Logstash.
* **Text Editor (Notepad++ / VS Code)**
     * For editing `.yml` configuration files.
* **Browser (Chrome / Firefox)**
     * To access the Kibana dashboard (`http://localhost:5601`).

#### By default, the stack exposes the following ports:

* Elasticsearch: 9200 (HTTP), 9300 (Transport)
* Kibana: 5601
* Logstash (default): 5044 (or other custom Beats/inputs)


> [!WARNING]
> Elasticsearch's [bootstrap checks][bootstrap-checks] were purposely disabled to facilitate the setup of the Elastic
> stack in development environments. For production setups, we recommend users to set up their host according to the
> instructions from the Elasticsearch documentation: [Important System Configuration][es-sys-config].


## Let's Get Started

> [!IMPORTANT]
> To begin, create a free Azure account. You may be required to add a credit card on file for verification purposes, but you will not be charged.


### Create Resource Group
1. **Open Azure**
2. **Search "Resource Group"**
3. **Select " + Create "**
4. **Create a Resource Group Name**

> [!NOTE]
> Your Resource Group is going to be a folder in the cloud for all of the work you create.

![Image](https://github.com/user-attachments/assets/54777ed2-b62c-496e-a350-bd1e6ec33fc2)



### Create a Virtual Network
1. **Open Azure**
2. **Search "Virtual Network"**
3. **Select " + Create "**
4. **Under *Project Details* select your resource group**
5. **Create a "Virtual Network" Name**

> [!NOTE]
> A Virtual Network (VNet) acts as your private network in the cloud. It allows your virtual machines and other resources to securely communicate with each other, the internet, and on-premises networks.

![Image](https://github.com/user-attachments/assets/82abbf52-e195-45ea-a654-b990fde798b9)


### Create a Virtual Machine
1. **Open Azure**
2. **Search "Virtual Machine"**
3. **Select " + Create "**
4. **Under *Project Details* select your resource group**
5. **Create a "Virtual Machine" Name**
6. **Under *"Image"* select "Windows 10 Pro"**
7. **Select a Size**
8. **Under *"Administrator Account"* create a username & password**
9. 


> [!NOTE]
> The Virtual Machine (VM) will serve as your honeypot — a purposely exposed system designed to attract and log real-world cyberattacks. This will allow you to analyze attacker behavior and collect data for security research.

![Image](https://github.com/user-attachments/assets/60809bf9-d88d-49f7-b6e3-356da923131d)
