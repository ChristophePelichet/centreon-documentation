---
id: hardware-storage-purestorage-restapi
title: Pure Storage
---

## Overview

Pure Storage develops flash-based storage for data centers using consumer-grade solid state drives. 
It provides proprietary de-duplication and compression software to improve the amount of data that can be stored on each drive. 
It also develops its own flash storage hardware.

## Plugin-pack assets

### Monitored objects

* Storage array

## Monitored metrics                                                                                                   

More information into Pure Storage official API documentation : https://blog.purestorage.com/introducing-the-pure1-rest-api/

<!--DOCUSAURUS_CODE_TABS-->
<!--Alarms-Global-->

| Metric name        | Description                                              |
| :----------------- | :------------------------------------------------------- |
| Status             | Status of alarms. Threshold/Unit: String                 |

<!--Hardware-Global-->

| Metric name        | Description                                             |
| :----------------- | :------------------------------------------------------ |
| Status             | Status of components. Threshold/Unit: String            |

<!--Volume-Usage-Global-->

| Metric name        | Description                                              |
| :----------------- | :------------------------------------------------------- |
| Volume-Usage       | The usage of volume. Unit: Bytes or %                    |
| Data-Reduction     | The data-reduction ratio on the volume. Unit: ratio      |
| Total-Reduction    | The total-reduction on the volume. Unit: count           |

<!--END_DOCUSAURUS_CODE_TABS-->

## Prerequisites

* This Monitoring Plugin requires at least a Pure Storage API version >= 1.11 (https://static.pure1.purestorage.com/api-swagger/index.html).

#### Create specific user

You have to configure the user who can connect to storage array. This user must have at least "read only" access to the storage array.
 
## Installation

<!--DOCUSAURUS_CODE_TABS-->

<!--Online IMP Licence & IT-100 Editions-->

1. Install the Plugin on every poller expected to monitor Pure Storage arrays:

```bash
yum install centreon-plugin-Hardware-Storage-Purestorage-Restapi
```

2. Install the "Pure-Storage-Restapi" Centreon Plugin Pack from the "Configuration > Plugin packs > Manager" page


<!--Offline IMP License-->

1. Install the Plugin on every poller expected to monitor Pure Storage resources:

```bash
yum install centreon-plugin-Hardware-Storage-Purestorage-Restapi
```

2. Install the Centreon Plugin Pack RPM on your central server:

```bash
yum install centreon-pack-hardware-storage-purestorage-restapi
```

3. Install the "Pure-Storage-Restapi" Centreon Plugin Pack from the "Configuration > Plugin packs > Manager" page

<!--END_DOCUSAURUS_CODE_TABS-->

## Host Configuration

Apply the "HW-Storage-Purestorage-Restapi-custom" template to your newly created host. Then fill the macros value fields marked as mandatory below: 

| Mandatory | Name            | Description                    |
| :-------- | :-------------- | :----------------------------- |
| X         | APIURLPATH      | Pure Storage API URL Path      |
| X         | APIURLUSERNAME  | Pure Storage API Username      |
| X         | APIURLPASSWORD  | Pure Storage API Password      |
|           | APIEXTRAOPTIONS | Pure Storage API Extra options |

## FAQ

#### How can I test my plugin through the CLI and what does the main command_line parameters mean ?

Once you've installed your monitoring plugin, you can use the centreon-engine user to test it:

```bash
/usr/lib/centreon/plugins//centreon_purestorage_restapi.pl
	--plugin=storage::purestorage::restapi::plugin
	--mode=volume-usage
	--hostname=192.168.1.1
	--api-path='/api/1.11'
	--username='centreon'
	--password='centreon' 
	--filter-name='.*'
	--units='%'
	--warning-usage=''
	--critical-usage=''
	--warning-data-reduction=''
	--critical-data-reduction=''
	--warning-total-reduction=''
	--critical-total-reduction=''
	--verbose

OK: Volume 'PROD::CENTREON' Usage Total: 6.00 TB Used: 1.13 TB (18.85%) Free: 4.87 TB (81.15%), Data Reduction : 2.917, Total Reduction : 5.193, Snapshots : 0.00 B
```

This command checks your storage array Pure Storage volume usage (```--mode=volume-usage```) using api url (```--api-path='/api/1.11'```). It offers detailed space usage as well as storage optimization ratios.
