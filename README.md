# How to use File Notification Message Distribution with the Client File Store (CFS) Service

## Overview

The Data Platform Client File Store (CFS) Service offers an alternative way to be notified when the new FileSet available through Amazon Simple Queue Service (SQS). The consumers do not need to polling request to fileset API, but continuously monitor the queue and received all required information (including file id) and able to download new file directly through file stream api instead.

This article is the part 2 of the [A Step-By-Step Workflow Guide for RDP Client File Store (CFS) API](https://developers.lseg.com/en/article-catalog/article/a-step-by-step-workflow-guide-for-rdp-client-file-store--cfs--ap) article. This article describes how to use the Data Platform's [Message services delivery API](https://developers.lseg.com/en/article-catalog/article/alerts-delivery-mechanism-in-rdp) to notified consumers (aka subscribers) when the new bulk file is available on the CFS API, and demonstrate with the ready-to-use example tool.

# <a id="whatis_rdp"></a>What is Refinitiv Data Platform (RDP) APIs?

The [Refinitiv Data Platform (RDP) APIs](https://developers.lseg.com/en/api-catalog/refinitiv-data-platform/refinitiv-data-platform-apis) provide various Refinitiv data and content for developers via easy-to-use Web-based API.

RDP APIs give developers seamless and holistic access to all of the Refinitiv content such as Environmental Social and Governance (ESG), News, Research, etc, and commingled with their content, enriching, integrating, and distributing the data through a single interface, delivered wherever they need it.  The RDP APIs delivery mechanisms are the following:
* Request - Response: RESTful web service (HTTP GET, POST, PUT or DELETE) 
* Alert: delivery is a mechanism to receive asynchronous updates (alerts) to a subscription. 
* Bulks:  deliver substantial payloads, like the end-of-day pricing data for the whole venue. 
* Streaming: deliver real-time delivery of messages.

This example project is focusing on the Request-Response: RESTful web service delivery method only.  

![figure-1](images/01_rdp.png "Refinitiv Data Platform content set")

For more detail regarding the Refinitiv Data Platform, please see the following APIs resources: 
- [Quick Start](https://developers.lseg.com/en/api-catalog/refinitiv-data-platform/refinitiv-data-platform-apis/quick-start) page.
- [Tutorials](https://developers.lseg.com/en/api-catalog/refinitiv-data-platform/refinitiv-data-platform-apis/tutorials) page.

## <a id="what_is_cfs"></a>What is CFS?

**Client File Store (CFS)** aka File Distribution is a capability of Refinitiv Data Platform (RDP) that provides authorization and enables access to content files stored in publisher-supplied repository. CFS defines content ownership that publisher are isolated. And subscribers can trust the source of content.

CFS is engineered as a self-service metadata tool intend for publishers and subscribers. CFS provides bucket and file-set to organize files to simplify the interaction with publishers or subscribers CFS doesn't store file directly. Actual files are store in publisher-supplied. AWS S3 only one type storage that supported by current CFS.


## <a id="what_is_msd"></a>What is Message Distribution Service?

The Message Distribution Service is a service that allows customers to subscribe to message queue-based interfaces to retrieve non-real-time content changes for all datasets. The Refinitiv Data Platform currently utilizes [AWS SQS](https://aws.amazon.com/sqs/) as a main message queue technology.

Integrated with Client File Store (CFS) Service, message distribution will deliver the message notification for end user clients when the client's interested bulk file is available. 

Note: 
- The equivalent message queue service on [Microsoft Azure](https://azure.microsoft.com/) is [Queue Storage](https://azure.microsoft.com/en-us/products/storage/queues/) service.
- The most equivalent message queue service on [Google Cloud Platform](https://cloud.google.com/) is [Pub/Sub](https://cloud.google.com/pubsub) service.

## <a id="how_file_noti_work"></a>How the File Notification Message Distribution work?

File Notification service lets the clients subscribes for the notification of their interested CFS file (using Bucket names, Package Id, etc) to the RDP Message Distribution Service. Then the clients continuously monitor the queue and received all required information, and able to download new file directly through the RDP file stream api when the file is available.

![figure-2](images/02_diagram.png "File Notification Message Distribution diagram")

### File Notification Message Distribution Workflow Step-By-Step

Let's drive into more technical detail about the File Notification Message Distribution workflow. The application steps are as follows:

1. Subscribers make a subscription to API https://api.refinitiv.com/message-services/v1/file-store/subscriptions endpoint by specified CFS content that they interest. E.g. Bucket Name, Package Id, Fileset Attribute.
2. Subscribers received 
    - Subscription ID
    - SQS Endpoint
    - Decryption Key 
3. Subscribers request credential to access the provided SQS.
4. Subscribers continually checking the SQS in short interval and as soon as the new FileSet that matched with the criteria available message will be available in user specific SQS
5. Fetch message from SQS and decrypt the message to see FileSet / File detail
6. Call CFS API to generate the pre-signed URL to download the file.

[TBD]

## <a id="references"></a>References

That brings me to the end of my File Notification Message Distribution with the CFS Service project. For further details, please check out the following resources:

* [Refinitiv Data Platform APIs page](https://developers.lseg.com/en/api-catalog/refinitiv-data-platform/refinitiv-data-platform-apis) on the [Refinitiv Developer Community](https://developers.lseg.com/) website.
* [Refinitiv Data Platform APIs Playground page](https://apidocs.refinitiv.com/Apps/ApiDocs).
* [Refinitiv Data Platform APIs: Introduction to the Request-Response API](https://developers.lseg.com/en/api-catalog/refinitiv-data-platform/refinitiv-data-platform-apis/tutorials#introduction-to-the-request-response-api).
* [Refinitiv Data Platform APIs: Authorization - All about tokens](https://developers.lseg.com/en/api-catalog/refinitiv-data-platform/refinitiv-data-platform-apis/tutorials#authorization-all-about-tokens).
* [Limitations and Guidelines for the RDP Authentication Service](https://developers.lseg.com/en/article-catalog/article/limitations-and-guidelines-for-the-rdp-authentication-service) article.
* [Getting Started with Refinitiv Data Platform](https://developers.lseg.com/en/article-catalog/article/getting-start-with-refinitiv-data-platform) article.
* [CFS API User Guide](https://developers.lseg.com/en/api-catalog/refinitiv-data-platform/refinitiv-data-platform-apis/documentation#cfs-api-user-guide).


For any questions related to Refinitiv Data Platform APIs, please use the [RDP APIs Forum](https://community.developers.refinitiv.com/spaces/231/index.html) on the [Developers Community Q&A page](https://community.developers.refinitiv.com/).


