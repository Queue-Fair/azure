---
## Queue-Fair Free Azure Virtual Waiting Room Network-Level Adapter README & Installation Guide

Queue-Fair can be added to any web server easily in minutes, and is a great way to get a free Azure virtual waiting room, as Queue-Fair offers its own Free Tier, and the Adapter only users Azure free tier features.  You will need a Queue-Fair account - please visit https://queue-fair.com/free-trial if you don't already have one.  You should also have received our Technical Guide.  To find out more about how a Virtual Waiting Room protects your site or app from traffic spikes, see https://queue-fair.com/virtual-waiting-room

## Client-Side JavaScript Adapter

Most of our customers prefer to use the Client-Side JavaScript Adapter, which is suitable for all sites that wish solely to protect against overload.

To add the Queue-Fair Client-Side JavaScript Adapter to your web server, you don't need the files included in this distribution.

Instead, add the following tag to the `<head>` section of your pages:
 
```
<script data-queue-fair-client="CLIENT_NAME" src="https://files.queue-fair.net/queue-fair-adapter.js"></script>`
```

Replace CLIENT_NAME with the account system name visibile on the Account -> Your Account page of the Queue-Fair Portal

You shoud now see the Adapter tag when you perform View Source after refreshing your pages.

And you're done!  Your queues and activation rules can now be configured in the Queue-Fair Portal.

## Azure Network-Leve Adapter
Using the Azure Adapter means that your Azure implementation communicates directly with the Queue-Fair Queue Server Cluster, rather than your visitors' browsers or your origin server.

This can introduce a dependency between our systems, which is why most customers prefer the Client-Side Adapter.  See Section 10 of the Technical Guide for help regarding which integration method is most suitable for you.

The Azure Adapter is a small JavaSrcript library that will run as a serverless Azure Function when visitors make requests served by Azure.

Due to ongoing issues with unscrupulous providers taking advantage of our intellectual property without permission, the source code and installation guide is currently available on request from Queue-Fair for new and existing clients.

It is adapted from our cross-platform Node Adapter - there are changes to the QueueFairService class, which is the one that usually contains platform-specific code, and also some small changes to the QueueFairAdapter class to use the Azure native httpRequest and crypto functions.  Unlike our https://github.com/queue-fair/node adapter, all the classes are defined in the one `main.js` file, and the QueueFairConfig class is replaced with a constant object.  It all works the same way.

The Adapter periodically checks to see if you have changed your Queue-Fair settings in the Portal, and caches the result in memory, but other than that if the visitor is requesting a page that does not match any queue's Activation Rules, it does nothing, and Azure will return a (possibly cached) copy of the page from your origin server(s).

If a visitor requests a page that DOES match any queue's Activation Rules, the Adapter consults the Queue-Fair Queue Servers to make a determination whether that particular visitor should be queued (Safe Mode, recommended) or sends the visitor to be counted at the Queue-Fair Queue Servers (Simple Mode).  If so, the visitor is sent to our Queue Servers and execution and generation of the page for that HTTP request for that visitor will cease, and your origin server will not receive a request.  If the Adapter determines that the visitor should not be queued, it sets a cookie to indicate that the visitor has been processed and Azure will return a page from its cache or contact your origin server as normal.

Thus the Azure Adapter prevents visitors from skipping the queue by disabling the Client-Side JavaScript Adapter, and also eliminates load on your origin server when things get busy.
