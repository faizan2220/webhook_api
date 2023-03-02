**Webhooks**

## Table of Contents

![](./media/image1.wmf)![](./media/image2.wmf)![](./media/image3.wmf)![](./media/image4.wmf)![](./media/image5.wmf)![](./media/image1.wmf)![](./media/image6.wmf)![](./media/image7.wmf)![](./media/image8.wmf)![](./media/image9.wmf)![](./media/image10.wmf)![](./media/image11.wmf)![](./media/image12.wmf)![](./media/image1.wmf)![](./media/image13.wmf)

**Overview**

-   [Product Benefits](#product-benefits)

-   [Usecases](#usecases)

-   [Component Parts & Microservices](#component-parts-microservices)

-   [Process Types](#process-types)

    -   [Product Onboarding](#product-onboarding)

    -   [Customer Onboarding](#customer-onboarding)

The webhooks service is a standalone, contained system deployed and
managed by BTIQ that provides segments with a simple way to create
subscriptions for their customers and send them event notifications.
BTIQ manages the microservices and databases, the segment only needs to
set up the subscriptions and use the BTIQ SDK library to send events,
and BTIQ takes care of the rest.

Webhooks is a service that allows Bottomline service and application to
notify customers, their applications, and even other Bottomline
applications when a relevant event has occurred. When a customer or a
disparate Bottomline application needs to be notified of an event in
real-time the webhook service plays a key role in building the right
solution. Utilizing the Webhooks service makes it easy to create
subscriptions and securely send event notifications directly to
customers via API calls. 

**        Figure: Webhooks - Subscription & Consumption**

        ![](./media/image14.png){style="width:4.875in;height:2.99306in"}

# Product Benefits

**For Segments**

-   One API call per customer to set up all of their event subscriptions

-   Easy to implement library to post events

-   The CRC process allows validating the connectivity with the
    customer's application

**For the end customer**

-   Secure way to be notified instantly about events they care about

-   Only subscribe to events that matter

# Usecases

**Subscribe**

Product lines can create and update subscriptions on behalf of their
customers. BTIQ generates a *secret *that is shared with the customer.
BTIQ uses the shared secret to compute a signature of the event payload
that customers can check to confirm that Bottomline is the source of the
incoming webhooks and ensure that the webhooks payload has not been
modified in transit (integrity check). The customer webhook URL must be
whitelisted. A single subscription can handle one or multiple event
types. An event type can be part of multiple subscriptions.

 How to request whitelisting a webhook URL: [Click
here](https://bottomlinetechnologies.sharepoint.com/:u:/r/sites/BTIQIntranet/SitePages/Request%20Access%20BTIQ%20Platform.aspx?csf=1&web=1&e=PwXj5s) (this
process can take a few days)

**Challenge Response Checks**

To verify that the customer is the owner of the webhook URL, LOBs can
trigger a Challenge Response Check (CRC) during their onboarding process
or any time they wish.

**Send events**

Applications can send events to the webhooks service, which notifies the
customer endpoints. An event can trigger 0, 1, or multiple notifications
depending on the customer's subscriptions. The Webhooks manage automatic
retries with a default behaviour (retry after 30sec, 5min, 30min, 2hrs,
and then stop) that can be changed for customer-specific needs (sum of
retries can't surpass 24hrs, can't exceed five retries).

**Exceptions management**

Product lines can get the list of notifications (for a given
subscription) with their related status. If a notification has a failed
status, it can trigger a retry.

# Component Parts & Microservices

Webhooks Service made up of:

1.  Subscription API

2.  Event Consumer API (exposed via the Webhook Library SDK)

3.  Subscription and Event database

4.  Event Notifier

# **Process Types**

## **Product Onboarding**

Product onboarding is a one-shot process done by the Bottomline product
& development team. The Bottomline development team implements the logic
of sending notifications whenever events occur (using Webhooks SDK). The
Bottomline product team documents the notifications to the customer and
the payload linked to those notifications.

*E.g., Whenever a payment is confirmed (event), we sends
the PaymentId \[String\], Date \[DateTime\], and Amount \[Number\] 
(format)*

## **Customer Onboarding**

Customer onboarding is a joint effort between the Bottomline
implementation team and each segment. The segment implements endpoint(s)
that receives and manages notifications.

*E.g. <https://customer1.com/payment-confirmed> manages payment
confirmations.*

The Bottomline (Segment) implementation team set up the subscriptions on
behalf of the customer.

*E.g. Payment confirmations for customer1 are sent
to <https://customer1.com/payment-confirmed>.*

The Bottomline implementation team (Segment and Shared Platform) and the
customer collaborate to share security artefacts and run tests.

**Architecture View**

-   [Logical View](#logical-view)

-   [Data View](#data-view)

-   [Deployment View](#deployment-view)

-   [Use case View](#use-case-view)

# **Logical View**

![](./media/image15.png){style="width:4.875in;height:2.97222in"}

# **Data View**

+-----------------------------------------------------------------------+
|   ------------------------------------------------------------------- |
|   ![](./media/image16.png){style="width:4.6875in;height:2.91667in"}   |
|   ------------------------------------------------------------------- |
|                                                                       |
|   ------------------------------------------------------------------- |
+=======================================================================+
+-----------------------------------------------------------------------+

# **Deployment View**

+-----------------------------------------------------------------------+
|   ------------------------------------------------------------------  |
|   ![](./media/image17.png){style="width:4.875in;height:5.09722in"}    |
|   ------------------------------------------------------------------  |
|                                                                       |
|   ------------------------------------------------------------------  |
+=======================================================================+
+-----------------------------------------------------------------------+

# **Use case View**

**Sequence Diagram**

[Send Notification]{.underline}

+-----------------------------------------------------------------------+
|   ------------------------------------------------------------------  |
|   ![](./media/image18.png){style="width:4.875in;height:2.59722in"}    |
|   ------------------------------------------------------------------  |
|                                                                       |
|   ------------------------------------------------------------------  |
+=======================================================================+
+-----------------------------------------------------------------------+

[Process Notification]{.underline}

+-----------------------------------------------------------------------+
|   ------------------------------------------------------------------  |
|   ![](./media/image19.png){style="width:4.875in;height:3.22222in"}    |
|   ------------------------------------------------------------------  |
|                                                                       |
|   ------------------------------------------------------------------  |
+=======================================================================+
+-----------------------------------------------------------------------+

**Product Onboarding**

Product Onboarding is a one-shot process **done by Segments** (Product &
Development team) willing to integrate webhook notifications in their
solution

![](./media/image20.png){style="width:4.875in;height:0.45833in"}

+-----+-----------------+---------+---------------------------------+
| **\ | **Step**        | **      | **Description**                 |
| #** |                 | Resp.** |                                 |
+=====+=================+=========+=================================+
| 1   | [Document       | PM/PO   | The product team documents the  |
|     | webhook events  |         | notifications to be sent to the |
|     | and payload     |         | customer and the payload linked |
|     | forma           |         | to those notifications          |
|     | t](#Bookmark20) |         |                                 |
|     |                 |         | *E.g. Whenever a payment is     |
|     |                 |         | confirmed (event), BTIQ sends   |
|     |                 |         | the PaymentId, Date and Amount  |
|     |                 |         | (payload)*                      |
|     |                 |         |                                 |
|     |                 |         | -   *Event: Payment Confirmed*  |
|     |                 |         |                                 |
|     |                 |         | -   *Payload : PaymentId        |
|     |                 |         |     (String), Date (DateTime),  |
|     |                 |         |     Amount (Number)*            |
+-----+-----------------+---------+---------------------------------+
| 2   | [Request for a  | Dev     | To be ready to consume Webhook  |
|     | service         |         | APIs, Segments need to get a    |
|     | accoun          |         | service account with relevant   |
|     | t](#Bookmark21) |         | permissions                     |
+-----+-----------------+---------+---------------------------------+
| 3   | [Implement      | Dev     | The dev team implements the     |
|     | logic of        |         | logic of sending notifications  |
|     | sending         |         | whenever events occur (using    |
|     | notification    |         | Webhooks SDK)                   |
|     | s](#Bookmark22) |         |                                 |
+-----+-----------------+---------+---------------------------------+
| 4   | [Test           | Dev     | The dev team to run end-to-end  |
|     | s](#Bookmark44) |         | tests                           |
+-----+-----------------+---------+---------------------------------+

[]{#Bookmark20 .anchor}**1 - Document webhook events and payload
format**

The LOBs product team documents the notifications to be sent to the
customer, and the payload linked to those notifications.

**Payload example: **

{\
  "type": "TEST_EVENT",\
  "payload": "{\\JSON Value\\:\\Test\\}",\
  "productId": "f3a96ba7-a3ef-43b1-b098-64872401cb95"\
}

[]{#Bookmark21 .anchor}**2 - Request service account**

To be ready to consume Webhooks APIs, a LOB needs to:

1.  Ask for a scopeId for each of their Datacenter/Environment, then
    make changes in their Identity Provider client configuration

2.  Ask for one or more service accounts for each of their scopeId

How to request a service account: [Click
Here](https://confluence.bottomline.tech/display/SPFM/Request+for+a+Service+Account)

[]{#Bookmark22 .anchor}**3 - Implement logic of sending notifications**

-   [Relevant APIs](#relevant-apis)

-   [Multitenancy](#multitenancy)

-   [Automatic Retries](#automatic-retries)

The LOB development team implements the logic of sending notifications
whenever events occur. The [Webhooks
SDK](https://confluence.bottomline.tech/display/SPFM/Webhooks+-+Misc#WebhooksSDK)
helps to implement the API calls.

# **Relevant APIs**

Relevant APIs to implement Webhooks in the product are:

+----+-----------------------------------+---------------------------+
| ** | **API**                           | **Description**           |
| Ty |                                   |                           |
| pe |                                   |                           |
| ** |                                   |                           |
+====+===================================+===========================+
| PO | /webhooks/events                  | Triggers a webhook event. |
| ST |                                   |                           |
|    |                                   | Attributes are:           |
|    |                                   |                           |
|    |                                   | -   Type                  |
|    |                                   |                           |
|    |                                   | -   Payload               |
|    |                                   |                           |
|    |                                   | -   ProductId (UUID) -    |
|    |                                   |     used for              |
|    |                                   |     subscriptions (Refer  |
|    |                                   |     to Customer           |
|    |                                   |     Onboarding section)   |
|    |                                   |                           |
|    |                                   | Webhooks expects an       |
|    |                                   | idempotency key in the    |
|    |                                   | request for this method - |
|    |                                   | For more information:     |
|    |                                   | [Click                    |
|    |                                   | Here](h                   |
|    |                                   | ttps://confluence.bottoml |
|    |                                   | ine.tech/display/SPFM/Web |
|    |                                   | hooks+-+Misc#Idempotency) |
|    |                                   |                           |
|    |                                   | BTIQ advise using the     |
|    |                                   | [outb                     |
|    |                                   | ox](https://confluence.bo |
|    |                                   | ttomline.tech/display/SPF |
|    |                                   | M/Webhooks+-+Misc#Outbox) |
|    |                                   | library to implement      |
|    |                                   | async calls and guarantee |
|    |                                   | delivery.                 |
+----+-----------------------------------+---------------------------+
| G  | /webhooks/subscription            | List all notifications    |
| ET | s/{subscription_id}/notifications | for a given subscription. |
|    |                                   |                           |
|    |                                   | List of objects having    |
|    |                                   | attributes:               |
|    |                                   |                           |
|    |                                   | -   Notification ID       |
|    |                                   |                           |
|    |                                   | -   Date created          |
|    |                                   |                           |
|    |                                   | -   Date updated          |
|    |                                   |                           |
|    |                                   | -   The latest HTTP code  |
|    |                                   |     received from the URL |
|    |                                   |     that the event was    |
|    |                                   |     sent to               |
|    |                                   |                           |
|    |                                   | -   Number of manual      |
|    |                                   |     retries               |
|    |                                   |                           |
|    |                                   | -   Type of event         |
|    |                                   |                           |
|    |                                   | -   ID of the related     |
|    |                                   |     tenant                |
|    |                                   |                           |
|    |                                   | -   ID of the related     |
|    |                                   |     product               |
+----+-----------------------------------+---------------------------+
| G  | /webhooks/events/{event_id}       | Gets all information      |
| ET |                                   | about a specific          |
|    |                                   | notification              |
|    |                                   |                           |
|    |                                   | -   Notification ID       |
|    |                                   |                           |
|    |                                   | -   Date when the object  |
|    |                                   |     was created           |
|    |                                   |                           |
|    |                                   | -   Date when the object  |
|    |                                   |     was updated           |
|    |                                   |                           |
|    |                                   | -   The latest HTTP code  |
|    |                                   |     received from the URL |
|    |                                   |     that the event was    |
|    |                                   |     sent to               |
|    |                                   |                           |
|    |                                   | -   Number of manual      |
|    |                                   |     retries               |
|    |                                   |                           |
|    |                                   | -   Type of event         |
|    |                                   |                           |
|    |                                   | -   ID of the related     |
|    |                                   |     tenant                |
|    |                                   |                           |
|    |                                   | -   ID of the related     |
|    |                                   |     product               |
|    |                                   |                           |
|    |                                   | -   ID of the related     |
|    |                                   |     subscription          |
|    |                                   |                           |
|    |                                   | -   Status of the event   |
|    |                                   |                           |
|    |                                   | -   Idempotency key       |
|    |                                   |                           |
|    |                                   | -   Number of failed      |
|    |                                   |     automatic retries     |
|    |                                   |                           |
|    |                                   | -   Latest state of the   |
|    |                                   |     event                 |
+----+-----------------------------------+---------------------------+

You may also consider implementing APIs related to Customer Onboarding
or Troubleshooting

-   Retry sending a notification - documented in the Customer Onboarding
    section

-   Challenge Response Check - documented in the Troubleshooting section

Full swagger documentation here
<https://btiq-stable.saas-n.com/webhooks/v3/api-docs>

# **Multitenancy**

All API calls require the unique id of the customer (tenantId)

-   If you are managing your organizations using the Shared Platform
    Organization service, the tenantId is the UUID of the organization

-   If you are managing them using your service, you need to ensure a
    unique UUID is linked to your customer's organization, and you have
    to use this UUID as the tenantId.

# **Automatic Retries**

When the customer API does not confirm the successful receipt of a
webhook event and if a successful **2XX** HTTP response code is not
received, then the webhook assumes the event was not sent and initiates
a retry mechanism.

Here is the context:

-   Whenever the product/solution posts an event to the customer who has
    subscribed to an eventType, provide the URL that must be called when
    an event occurs.

-   Webhooks service finds the subscription(s) related to the event type
    and creates a notification for each subscription. Then it sends the
    notification to the customer endpoint and updates it by setting the
    HTTP status returned by the customer endpoint.

-   When the webhooks service doesn't receive an acknowledgement from
    the customer API for the sent notification, it's re-attempted after
    a predefined time following the retry mechanism.

**When do we retry?\
**We retry when we do not receive a successful response from the
destination server.

Below is a list of HTTP status codes that we consider for the retries:

**Response        Retry**\
4xx                 
 ![(tick)](./media/image21.png){style="width:0.33333in;height:0.33333in"}\
5xx                 
 ![(tick)](./media/image21.png){style="width:0.33333in;height:0.33333in"}\
Timed Out     
 ![(tick)](./media/image21.png){style="width:0.33333in;height:0.33333in"}

**3.1 - Examples of Webhooks Requests and Responses**

### Events

This is an object representing an event you send to the webhooks
subscription.

  **Endpoints**
  -----------------------------
  /webhooks/1/webhooks/events

### The event object.

#### Attributes

+-------+-----------------+------------------------------------------+
| type  | The event type  | String type                              |
|       |                 |                                          |
|       |                 | This must be the same as defined in the  |
|       |                 | target subscription.                     |
+=======+=================+==========================================+
| pa    | The payload of  | JSON type                                |
| yload | an event        |                                          |
+-------+-----------------+------------------------------------------+
| prod  | The ID of a     | UUID type                                |
| uctId | product         |                                          |
|       |                 | This must be the same as defined in the  |
|       |                 | target subscription.                     |
+-------+-----------------+------------------------------------------+

The event object.

{

"type": "TEST_EVENT",

"payload": "{\\JSON Value\\:\\Test\\}",

"productId": "f3a96ba7-a3ef-43b1-b098-64872401cb95"

}

### Post event

**Parameters**

1.  The event object in the body.

2.  {

3.  "type": "TEST_EVENT",

4.  "payload": "{\\JSON Value\\:\\Test\\}",

5.  "productId": "f3a96ba7-a3ef-43b1-b098-64872401cb95"

> }

6.  'X-Idempotency-Key' HTTP header in UUID format to ensure the
    idempotency of an operation.

-   This is a required parameter for this particular request. To see
    details specific to webhooks use of the idempotency library:  [Click
    Here](http://Miscellaneous)

**Returns**

Empty body.

**Endpoint**

  POST /webhooks/1/webhooks/events
  ----------------------------------

#### Request Headers

X-On-Behalf-Of {tenantId}?

**Response**

Empty body

### Notifications

This is an object representing the state of the event you've sent to the
webhooks subscription.

  **Endpoints**
  ------------------------------------
  /webhooks/1/webhooks/notifications

### The notifications object.

#### Attributes

+-----------+---------------------------+----------------------------+
| id        | Notification ID           | UUID type                  |
+===========+===========================+============================+
| created   | Date when the object was  | Date type                  |
|           | created                   |                            |
+-----------+---------------------------+----------------------------+
| updated   | Date when the object was  | Date type                  |
|           | updated                   |                            |
+-----------+---------------------------+----------------------------+
| httpCode  | The latest HTTP code      | Integer type               |
|           | received from the URL     |                            |
|           | that the event was sent   |                            |
|           | to                        |                            |
+-----------+---------------------------+----------------------------+
| n         | The number of manual      | Integer type               |
| umRetries | retries                   |                            |
+-----------+---------------------------+----------------------------+
| eventType | Type of event             | String type                |
+-----------+---------------------------+----------------------------+
| tenantId  | The ID of a tenant        | UUID type                  |
+-----------+---------------------------+----------------------------+
| productId | The ID of a product       | UUID type                  |
+-----------+---------------------------+----------------------------+
| subsc     | The ID of a subscription  | UUID type                  |
| riptionId |                           |                            |
+-----------+---------------------------+----------------------------+
| status    | Status of event           | String enum type           |
|           |                           |                            |
|           |                           | Valid values: "Ready to be |
|           |                           | sent" or "Sent"            |
+-----------+---------------------------+----------------------------+
| idemp     | Idempotency key           | UUID type                  |
| otencyKey |                           |                            |
+-----------+---------------------------+----------------------------+
| failCount | The number of failed      | Integer type               |
|           | automatic retries         |                            |
+-----------+---------------------------+----------------------------+
| state     | Latest state of the event | String enum type           |
|           |                           |                            |
|           |                           | Valid values: "NEW",       |
|           |                           | "PROCESSING", "COMPLETED", |
|           |                           | or "ERROR"                 |
+-----------+---------------------------+----------------------------+

The notification object

{

"id": "4a1ee7b9-291a-4d6b-a2cb-d3aa276596aa",

"created": "2022-08-31T12:55:01.271493",

"updated": "2022-08-31T12:55:31.346895",

"httpCode": 400,

"numRetries": 0,

"eventType": "TEST_EVENT",

"tenantId": "a138f0fb-7df8-4b77-9632-741b71e4b2fc",

"productId": "f3a96ba7-a3ef-43b1-b098-64872401cb95",

"subscriptionId": "d49cc797-06a4-4bb6-b2f9-3c96db6110bc",

"status": "Sent",

"idempotencyKey": "be794020-eab9-42b4-b15a-ca7baec37e4a",

"failCount": 4,

"state": "ERROR"

}

### List notifications for subscription

**Parameters (set in the URL)**

+----+----------------------+----------------------------------------+
| Pa | Starting page        | The default value is 0                 |
| ge |                      |                                        |
+====+======================+========================================+
| Si | Number of elements   | The default value is 50                |
| ze | on a page            |                                        |
+----+----------------------+----------------------------------------+
| So | Sorting parameters   | Default sorting is by                  |
| rt |                      |                                        |
|    |                      | 'outboxEvent.lastModifiedDate' field   |
|    |                      | in descending order                    |
+----+----------------------+----------------------------------------+

**Returns**

List of notifications

**Endpoint**

  -----------------------------------------------------------------------
  GET /webhooks/1/webhooks/subscriptions/{subscription_id}/notifications
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

**Response**

{

"\_embedded": {

"notifications": \[

{

"id": "4a1ee7b9-291a-4d6b-a2cb-d3aa276596aa",

"created": "2022-08-31T12:55:01.271493",

"updated": "2022-08-31T12:55:31.346895",

"httpCode": 400,

"numRetries": 0,

"eventType": "uYUSapd",

"tenantId": "a138f0fb-7df8-4b77-9632-741b71e4b2fc",

"productId": "f3a96ba7-a3ef-43b1-b098-64872401cb95"

},

{

"id": "ea3e56b4-79ad-4279-abad-4c9c1b168927",

"created": "2022-08-31T12:54:34.927443",

"updated": "2022-08-31T12:55:04.582202",

"httpCode": 400,

"numRetries": 0,

"eventType": "uYUSapd",

"tenantId": "a138f0fb-7df8-4b77-9632-741b71e4b2fc",

"productId": "f3a96ba7-a3ef-43b1-b098-64872401cb95"

\]

},

"\_links": {

"self": {

"href":
"https://btiq-unstable.saas-n.com/webhooks/webhooks/subscriptions/d49cc797-06a4-4bb6-b2f9-3c96db6110bc/notifications?page=0&size=50&sort=outboxEvent.lastModifiedDate,desc",

"title": ""

}

},

"page": {

"size": 50,

"totalElements": 2,

"totalPages": 1,

"number": 0

}

}

### Get notification

**Parameters (set in the URL)**

  id   The ID of notification   UUID type
  ---- ------------------------ -----------

**Returns**

Notification object.

**Endpoint**

  GET /webhooks/1/webhooks/notifications/{notification_id}
  ----------------------------------------------------------

**Response**

{

"id": "4a1ee7b9-291a-4d6b-a2cb-d3aa276596aa",

"created": "2022-08-31T12:55:01.271493",

"updated": "2022-08-31T12:55:31.346895",

"httpCode": 400,

"numRetries": 0,

"eventType": "uYUSapd",

"tenantId": "a138f0fb-7df8-4b77-9632-741b71e4b2fc",

"productId": "f3a96ba7-a3ef-43b1-b098-64872401cb95",

"subscriptionId": "d49cc797-06a4-4bb6-b2f9-3c96db6110bc",

"status": "Sent",

"idempotencyKey": "be794020-eab9-42b4-b15a-ca7baec37e4a",

"failCount": 4,

"state": "ERROR"

}

### Errors

### Error object

{

"type": "http://api.bottomline.com/errors/validation",

"title": "A bad input from the caller triggered a \\bad request\\
response.",

"detail": "Error occurred when validating request body",

"status": 400,

"invalid-params": \[

{

"name": "events",

"reason": "field 'events' is mandatory and cannot be empty"

},

{

"name": "url",

"reason": "field 'url' is mandatory and cannot be blank"

}

\]

}

May describe one or more errors.

**HTTP status code summary**

  ----------------------------------------------------------------------
  **Code**       **Reason**
  -------------- -------------------------------------------------------
  200 - OK       Everything worked as expected

  400 - Bad      The request was unacceptable, often due to missing or
  Request        invalid parameter

  401 -          Invalid authorization
  Unauthorized   

  403 -          The user doesn't have permission to operate
  Forbidden      

  404 - Not      The requested resource doesn't exist
  Found          
  ----------------------------------------------------------------------

**3.2 - Webhooks Common 2.0 Error Handling**

### Errors description

Webhooks use the Common 2.0 error handling library and return a standard
error object in case of errors.

The structure of the object is:

1.  Type ("<http://api.bottomline.com/errors/validation>")

2.  Title ("A bad input from the caller triggered a \\bad request\\
    response.")

3.  Detailed description

4.  HTTP Status code (usually 400 - Bad Request)

5.  Name of the invalid parameter(s)

**The error object:**

{

"type": "http://api.bottomline.com/errors/validation",

"title": "A bad input from the caller triggered a \\bad request\\
response.",

"detail": "Invalid UUID specified for subscriptionId",

"status": 400,

"invalid-params": \[

{

"name": "subscriptionId",

"reason": "Invalid UUID specified"

}

\]

}

### Possible descriptions

#### **Attributes**

  -----------------------------------------------------------------------
  **Error description**      **Root cause**
  -------------------------- --------------------------------------------
  Field {} is mandatory      The mandatory field is missing in the
                             request (usually product id or tenant id)

  Field {} cannot be updated Different value for the read-only field is
                             provided in the update operation

  field {} is mandatory and  The field value cannot be blank
  cannot be blank            

  field {} is mandatory and  The field value cannot be empty
  cannot be empty            

  field {} must be equal or  Field value must be in boundaries
  greater than {}            

  field {} must be equal or  Field value must be in boundaries
  less than {}               

  Error occurred when        Invalid JSON provided in the request
  validating request body    

  Invalid UUID specified for Invalid UUID provided in the body
  {}                         (subscription id, product id etc.)

  Invalid value in           Wrong value in pagination parameters
  pagination parameters      (e.g. in the 'List subscriptions) operation)

  Payload must be a valid    Invalid JSON provided in the 'payload' field
  JSON                       during the 'Post event' operation

  The body of the request    Invalid (or empty) JSON is provided in the
  could not be understood by request body
  the server                 

  The sum of retry back-offs When creating or updating a subscription
  can not exceed 24 hours    retry interval must not exceed 24 hours

  The value should be equal  When creating or updating a subscription
  to or greater than 0.1     first retry value must be equal to or
  seconds                    greater than 0.1 seconds
  -----------------------------------------------------------------------

**Example:**

{

"type": "http://api.bottomline.com/errors/validation",

"title": "A bad input from the caller triggered a \\bad request\\
response.",

"detail": "The body of the request could not be understood by the
server",

"status": 400

}

### Names of invalid parameter(s)

Names of parameters are added to the "invalid-params" value to narrow
down pinpointing invalid values.

There can be one or more entries there.

**Example**

{

"type": "http://api.bottomline.com/errors/validation",

"title": "A bad input from the caller triggered a \\bad request\\
response.",

"detail": "Error occurred when validating request body",

"status": 400,

"invalid-params": \[

{

"name": "retryExponent",

"reason": "field 'retryExponent' must be equal or greater than 2"

},

{

"name": "maxRetries",

"reason": "field 'maxRetries' must be equal or greater than 0"

}

\]

}

[]{#Bookmark44 .anchor}**4 - Tests**

-   [Prerequisite](#prerequisite)

-   [Create Subscription](#create-subscription)

-   [Communicate shared secret to
    WTS](#communicate-shared-secret-to-wts)

-   [Postman Collection](#postman-collection)

The Shared Platform provides a Webhooks Testing Service (WTS) that
allows testing implementation of webhooks. The WTS simulates endpoints
that the customer implements.  It is a central service deployed and
managed by the Shared Platform Team.

  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **env**     **webhooks testing URL**
  ----------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  stable      [https://webhooks-testing-service-stable.btiq-ny2-integration-webhooks.svc.cluster.local:8543](https://webhooks-testing-service-stable.btiq-ny2-integration-webhooks.svc.cluster.local:8543/)

  staging -   [https://webhooks-testing-service-staging.btiq-ny2-staging-webhooks.svc.cluster.local:8543](https://webhooks-testing-service-staging.btiq-ny2-staging-webhooks.svc.cluster.local:8543/)
  us          

  staging -   [https://webhooks-testing-service-staging.btiq-gb03-staging-webhooks.svc.cluster.local:8543](https://webhooks-testing-service-staging.btiq-ny2-staging-webhooks.svc.cluster.local:8543/)
  uk          

  staging -   [https://webhooks-testing-service-staging.btiq-ch01-staging-webhooks.svc.cluster.local:8543](https://webhooks-testing-service-staging.btiq-ny2-staging-webhooks.svc.cluster.local:8543/)
  ch          
  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# **Prerequisite**

To onboard a test customer, refer to the Customer Onboarding section.

The purpose is to:

-   have a tenantId for the test customer

-   have an encryption key for the test customer for webhooks

# **Create Subscription**

LOBs must create a subscription for one or multiple events defined in
your application. The process is different depending if LOB wants to
test with or without non-repudiation. 

-   Without non-repudiation, the end-point URL to define the
    subscription is /webhooks

-   With non-repudiation, the end-point URL to define the subscription
    is /webhooks/non-repudiation

# **Communicate shared secret to WTS**

**Shared secret**

If the subscription is not defined with non-repudiation, you must
communicate the shared secret to the WTS.

For that, it has a dedicated endpoint: PUT /webhooks/secret - **TO BE
DOCUMENTED**

When an event is triggered, it is signed by webhooks by the same shared
secret and posted to subscriber - WTS endpoint POST /webhooks. WTS
validates requests and based on the result, returns 200 or 400 (if not
valid).

Validation takes two steps:

-   Is the signature expired - It should be 5min or less

-   Is the signature valid

**Signing certificate**

If the subscription is defined with non-repudiation, the WTS already has
the signing certificate configured, so action for Segments is required.

# **Postman Collection**

The Postman Collection below is the setup to be run in a stable
(Integration Testing) environment.

LOB need to obtain the client ID and secret from the vault. Please reach
out to the support channel to obtain credentials. The webhooks endpoints
are secured using the OAuth 2.0 Client Credentials grant type. You also
need a tenant ID. This is your Org ID and should be passed in using the
"X-On-Behalf-Of" header value in your requests where applicable.

[webhooks.postman_collection.json](https://confluence.bottomline.tech/download/attachments/352291702/webhooks.postman_collection.json?version=1&modificationDate=1664871806000&api=v2)

**Customer Onboarding**

Customer Onboarding is a joint effort between BT teams (Segment
Implementation Team, Cloud/System Engineering, Shared Services Team) and
**[each]{.underline}** customer

![](./media/image22.png){style="width:4.875in;height:0.79167in"}

+---+--------+-----------+----------------------------------------------+
| 1 | [Ask   | Segment   | The segment implementation team raises a     |
|   | for a  | imple     | Jira ticket to ask for a service account.    |
|   | s      | mentation | This is only required for the communication  |
|   | ervice | team      | between the segment service and the          |
|   | acc    |           | webhooks. Therefore this is only a one time  |
|   | ount]( |           | activity when the first customer is getting  |
|   | #Bookm |           | on-boarded.                                  |
|   | ark50) |           |                                              |
+===+========+===========+==============================================+
| 2 | [      | Segment   | As webhooks is a multi-tenancy organization, |
|   | Create | imple     | each customer must have                      |
|   | an     | mentation |                                              |
|   | organi | team      | -   a unique identifier representing their   |
|   | zation |           |     organization - Call Customer Hub to      |
|   | and    |           |     create Organization                      |
|   | encr   |           |                                              |
|   | yption |           | -   an encryption key to store sensitive     |
|   | keys]( |           |     data at rest - Call KMS service to       |
|   | #Bookm |           |     create encryption keys.                  |
|   | ark51) |           |                                              |
+---+--------+-----------+----------------------------------------------+
| 3 | [R     | Segment   | Whitelist Domain:                            |
|   | equest | imple     |                                              |
|   | to     | mentation | The segment implementation team raises a     |
|   | whi    | team /    | Jira ticket to Whitelist domain:             |
|   | telist | Shared    |                                              |
|   | cu     | Services  | -   Whitelist customer URLs - the shared     |
|   | stomer | / Cloud   |     platform team will liaise with the       |
|   | URLs & | En        |     Cloud/System Engineering \               |
|   | P      | gineering |     This takes close to a week to get it to  |
|   | orts]( |           |     proxy server                             |
|   | #Bookm |           |                                              |
|   | ark54) |           | NOTE: There is a [Whitelist                  |
|   |        |           | Ticket](https                                |
|   |        |           | ://jira.bottomline.tech/browse/BTIQ-13939),  |
|   |        |           | Once completed the script will take care of  |
|   |        |           | whitelisting. In which case the wait time    |
|   |        |           | for the whitelisting to Proxy Server is less |
|   |        |           | than 1 day.                                  |
|   |        |           |                                              |
|   |        |           | Whitelist Port:                              |
|   |        |           |                                              |
|   |        |           | There is a need to whitelist the port        |
|   |        |           | allowed for outbound communication. If it is |
|   |        |           | different from the supported Ports,          |
|   |        |           | supported ports are listed                   |
|   |        |           | [here](https://bit                           |
|   |        |           | bucket.bottomline.tech/projects/PUP/repos/co |
|   |        |           | ntrolrepo/browse/hieradata/product/inf.yaml? |
|   |        |           | at=c1607fe2f70e2159df77641a17c948c3e51e68bb) |
|   |        |           | Encourage Customer to use existing supported |
|   |        |           | Ports if possible, as this will eliminate    |
|   |        |           | need for the same.                           |
+---+--------+-----------+----------------------------------------------+
| 4 | [Setup | Shared    | -   Webhooks will need customer certs to     |
|   | mTLS   | Services  |     ensure integrity of the customer         |
|   | (optio |           |     webhooks listener URL. For Webhooks to   |
|   | nal)]( |           |     trust the customer certs, the customer   |
|   | #Bookm |           |     certificates can be retrieved either     |
|   | ark55) |           |     from the whitelisted URL or can be       |
|   |        |           |     shared by the customer.                  |
|   |        |           |                                              |
|   |        |           | > Please raise a ticket to on-board the      |
|   |        |           | > customer certificates to                   |
|   |        |           | > vault. [BTIQ-4709](https://j               |
|   |        |           | ira.bottomline.tech/browse/BTIQ-4709) - BTIQ |
|   |        |           | > Product/LOB Registration Request:          |
|   |        |           | > \[Product/LOB name\] - \[Purpose\]         |
|   |        |           |                                              |
|   |        |           | -   Customer can opt for mTLS. For mTLS,     |
|   |        |           |     webhooks uses the certs issued by        |
|   |        |           |     DigiCert. As Digicert is trusted across  |
|   |        |           |     the globe by all the HTTP clients,       |
|   |        |           |     therefore customers do not have to take  |
|   |        |           |     any specific action. If required we can  |
|   |        |           |     share the DigiCert CA from               |
|   |        |           |     [here]                                   |
|   |        |           | (https://confluence.bottomline.tech/download |
|   |        |           | /attachments/365167404/digicert-ca.crt?versi |
|   |        |           | on=1&modificationDate=1672814962000&api=v2). |
|   |        |           |     (Valid until **Apr 13 23:59:59 2031      |
|   |        |           |     GMT**)                                   |
+---+--------+-----------+----------------------------------------------+
| 5 | [      | Segment   | Segment implementation team setup the        |
|   | Create | imple     | subscriptions on behalf of the customer      |
|   | sub    | mentation |                                              |
|   | script | team      | *    E.g. Payment confirmations for          |
|   | ions]( |           | customer1 will be sent to                    |
|   | #Bookm |           | https://customer1.com/payment-confirmed*     |
|   | ark61) |           |                                              |
|   |        |           | The customer can choose between shared       |
|   |        |           | secret and non repudiation. Details about    |
|   |        |           | the shared secret and non-repudiation can be |
|   |        |           | found here.  Depending on the choice, BT     |
|   |        |           | implementation team sends the shared secret  |
|   |        |           | or the signing certificate to the customer   |
|   |        |           |                                              |
|   |        |           | Non-repudiation certificates are available   |
|   |        |           | based on the environment [here](#Bookmark88) |
+---+--------+-----------+----------------------------------------------+
| 6 | Imp    | Customer  |                                              |
|   | lement |           |                                              |
|   | We     |           |                                              |
|   | bhooks |           |                                              |
|   | Li     |           |                                              |
|   | stener |           |                                              |
+---+--------+-----------+----------------------------------------------+
| 7 | [T     | Segment   |                                              |
|   | ests]( | imple     |                                              |
|   | #Bookm | mentation |                                              |
|   | ark81) | team      |                                              |
+---+--------+-----------+----------------------------------------------+

[]{#Bookmark50 .anchor}**1 - Ask for a service account**

The Segment implementation team needs a service account to onboard new
customers (create an organization, users and roles, and make
subscriptions on behalf of the customer). This is only required for the
first customer, then this service account will be used for all the
following customers. 

How to request for a service account is documented here: [Request for a
Service
Account](https://confluence.bottomline.tech/display/SPFM/Request+for+a+Service+Account)

*Note: the service account for the Segments implementation team shall be
different from the service account used for Product Onboarding as they
don't require the same permissions*

[]{#Bookmark51 .anchor}**2 - Create an organization and encryption
keys**

-   [Creating Organization](#creating-organization)

-   [Creating Encryption Keys](#creating-encryption-keys)

As webhooks is a multi-tenancy organization, each LOB must have the
following:

-   a unique identifier

-   an encryption key to store sensitive data at rest

# **Creating Organization**

If you rely on Shared Platform Organization service to manage your
customers' organization, you can refer to the
[Organizations](https://confluence.bottomline.tech/display/SPFM/Organizations)
page, where you will find all relevant APIs on how to create and update
the organization

-   the tenantId to use for API calls is the UUID of the organization

If you rely on your solution, you need to ensure there is a unique UUID
linked to your customers' organization

-   the tenantId to use for API calls will be this UUID

# **Creating Encryption Keys**

As the webhooks service needs to store sensitive data (the payload)
encrypted at rest, you need to create an encryption key for webhooks for
each customer. You use the Shared Platform
[KMS](https://confluence.bottomline.tech/display/SPFM/Key+Management) to
create encryption keys.

Encryption keys for Webhooks follow this format:

       btiq-{environment Name}\_webhooks\_{Scope ID}\_{Organization
Id}\_encryption

Where

-   {environment Name} can be stable, staging or production

-   {Scope ID} has been provided to you by the Shared Platform Team
    during product onboarding

-   {Organization Id} is the tenant id (the unique UUID of your
    customer)

**[Alternative option]{.underline}**

Segments can ask the Shared Platform team to create encryption keys by
raising a Jira ticket (in the same ticket where they request a service
account and whitelist the customer URLs)

[Ask for a service account, and whitelist the customer URL](#Bookmark54)

[]{#Bookmark54 .anchor}**3 - Request to whitelist customer URL**

The Segment implementation team must raise a Jira ticket to whitelist
customer URLs - the Shared Platform team liaise with the Cloud/System
Engineering. This takes close to a week to get it to the proxy server.

**REQUEST PROCESS**

The Segment requesting access to Core Services APIs has to
CLONE [BTIQ-4709](https://jira.bottomline.tech/browse/BTIQ-4709) & rename
the Summary to include the *Product or Segment name and the purpose of
the request.*** **

**Please do not overwrite BTIQ-4709. **Only clone it before making your
changes.

Please assign your ticket to [Sreekanth
Chavaly](https://confluence.bottomline.tech/display/~Sreekanth.Chavaly) 

The requestor has to edit the ticket description to fill the scope of
his request.

-   **Purpose**: Purpose of the request (*"whitelist webhooks URLs")*

-   **Line Of Business**: The name of the Segment which is raising the
    request (*e.g.* *PTX, FM, CFRM, PMX*)

-   **Contact Email**: The email address for the contact

The Jira ticket will be assigned to a product owner who will ensure it
will be planned in the next sprint or sooner, depending on the priority
and workload. 

+-----------+---------------------------------------------------------+
| **        | The requestor edits the Ticket description to fill in   |
| WHITELIST | the details of the "Whitelist Webhook URLs" panel.      |
| WEBHOOK   |                                                         |
| URLs**    | -   **Operation**: Create, Delete                       |
|           |                                                         |
|           | -   **Customer URL/Domain**: The URL or domain name of  |
|           |     the URL the subscription will be going to           |
|           |                                                         |
|           | -   **Environments**: List of the environments that     |
|           |     will require access to the URL *(e.g. All, UK/QA,   |
|           |     US-NY2/Prod)*​​​​​​​                                       |
|           |                                                         |
|           | The Shared Platform team will add a "Security_Review"   |
|           | label to the ticket, and a security team member will    |
|           | review the domain and comment regarding approval or     |
|           | rejection. BTIQ DevOps updates the outgoing proxy to    |
|           | allow traffic to the customer URL upon approval.        |
+===========+=========================================================+
+-----------+---------------------------------------------------------+

[]{#Bookmark55 .anchor}**4 - Setup mTLS**

-   [Apache HTTP Client](#apache-http-client)

-   [Secret material](#secret-material)

-   [Testing](#testing)

    -   [Unit Testing](#unit-testing)

    -   [Integration Testing](#integration-testing)

Mutual TLS (mTLS) is a method for mutual authentication. mTLS ensures
that the parties at each end of a network connection are who they claim
to be by verifying that they both have the correct private key. The
information within their respective TLS certificates provides additional
verification. mTLS is often used in a Zero Trust security framework\* to
verify an organization's users, devices, and servers. It can also help
to keep APIs secure.

\*Zero Trust means that no user, device, or network traffic is trusted
by default, an approach that helps eliminate many security
vulnerabilities

Webhooks use mTLS protocol on the HTTP connection between itself and the
subscriber. The connection between webhooks and URL host from the
subscription should support mTLS.

As an HTTP connector with mTLS, it uses Apache HTTP Client.

**DOCUMENT HERE**

-   **HOW BT AND THE CUSTOMER MANAGE THEIR CERTIFICATES**

-   **HOW they can get each other certificate**

-   **WHAT THEY NEED TO DO WITH THE CERTIFICATE THEY RECEIVE (some of
    this is certainly documented below... to be checked)**

# **Apache HTTP Client**

Webhooks like many other services, use it by adding dependency of
required starter into pom.xml. More details about the client and its
usage can be found here: [Apache Http
Client](https://confluence.bottomline.tech/display/BTIQ/Http+Client#HttpClient-ApacheHttpClient).

The Webhooks HTTP client configuration:

client:

pool:

max-connections: 1024

max-connections-per-host: 1024

\# Connect timeout (ms) and socket read/write timeout (ms) exposed
through config map

\# Keep-alive connection after a response (0 = closed immediately)

default-keep-alive: 0

idle-connections-autoclose: true

idle-connections-timeout: 3600

idleConnectionsAutoClose: true

attribute "client.pool.max-connections" is required to turn
auto-configuration for the HTTP client.

HTTP client creates SSL context based on configuration:

server:

\# Below given are the ssl properties keystore and truststore to be
generated accordingly

ssl:

enabled: true

enabled-protocols: TLSv1.2

protocol: TLSv1.2

ciphers:
TLS_DHE_RSA_WITH_AES_256_GCM_SHA384,TLS_DHE_RSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256

key-store: file:keystore.jks

key-store-type: JKS

trust-store: file:truststore.jks

trust-store-type: JKS

client-auth: need

Secret materials for key and trust stores are imported by specific
scripts from the base image.  It requires certain crt and key files and
can import extra secret material files if present. See [Docker Base
Image](https://confluence.bottomline.tech/display/BTIQ/Docker+Base+Image#DockerBaseImage-ImportsecretmaterialsintoJKSfiles)
for more details.

Initialized by autoconfiguration bean (httpClient) is injected in the
constructor of WebhookServiceImpl and is used to post data to the
subscriber:

public int postToSubscriber(EventModel eventModel, String url, String
sharedSecret, String idempotencyKey, EventLogMetadata eventLogMetadata,
boolean isNonRepudiation) {

try {

String payload = eventModel.getPayload();

final var request = Request.Post(url)

.bodyString(payload, ContentType.APPLICATION_JSON)

.addHeader(IDEMPOTENCY_KEY_NAME, idempotencyKey);

...

final HttpResponse httpResponse =
Executor.newInstance(httpClient).execute(request).returnResponse();

...

}

# **Secret material**

As mentioned above base image requires service.key and service.crt files
to import to keystore.jks.  It also requires saas-ca-chain.crt,
consul_ca_root.crt, consul_ca_intermediate.crt to import to
truststore.jks. Jks files are used to create SSL context, and establish
mTLS connection by HTTP Client (see above). Secret material (private
keys, public keys, CAs) securely persisted in the Vault. Webhooks gitops
has a vault template to import vault data to files during pod
initialization.

Here how it looks like in the integration table
(applications/webhooks/overlays/integration-stable/webhooks-vault-config.yaml):

\#

\# internal CAs

\#

template {

contents = \<\<EOH

{{ with secret "saas_ca/issue/btiq_servers"
"common_name=webhooks-stable.btiq-ny2-integration-webhooks.svc.cluster.local"
"alt_names=\*.btiq-ny2-integration-webhooks.svc.cluster.local,\*.saas-n.com,\*.bottomline.com"
"ip_sans=127.0.0.1" -}}{{ .Data.issuing_ca }}{{ end }}

EOH

destination = "/vault/secrets/saas-ca-chain.crt"

}

template {

contents = \<\<EOH

{{ with secret "kv/data/btiq/btiq-ny2-integration-webhooks" -}}{{
.Data.data.consulCaRoot }}{{- end }}

EOH

destination = "/vault/secrets/consul_ca_root.crt"

}

template {

contents = \<\<EOH

{{ with secret "kv/data/btiq/btiq-ny2-integration-webhooks" -}}{{
.Data.data.consulCaIntermediate }}{{- end }}

EOH

destination = "/vault/secrets/consul_ca_intermediate.crt"

}

\#

\# internal crt/pk

\#

template {

contents = \<\<EOH

{{ with secret "saas_ca/issue/btiq_servers"
"common_name=webhooks-stable.btiq-ny2-integration-webhooks.svc.cluster.local"
"alt_names=\*.btiq-ny2-integration-webhooks.svc.cluster.local,\*.saas-n.com,\*.bottomline.com"
"ip_sans=127.0.0.1" -}}{{ .Data.certificate }}

{{ .Data.issuing_ca }}{{ end }}

EOH

destination = "/vault/secrets/service.crt"

}

template {

contents = \<\<EOH

{{ with secret "saas_ca/issue/btiq_servers"
"common_name=webhooks-stable.btiq-ny2-integration-webhooks.svc.cluster.local"
"alt_names=\*.btiq-ny2-integration-webhooks.svc.cluster.local,\*.saas-n.com,\*.bottomline.com"
"ip_sans=127.0.0.1" -}}{{ .Data.private_key }}{{ end }}

EOH

destination = "/vault/secrets/service.key"

}

For lower environments, secret material above is used for mTLS and that
is enough.

For higher environments (prod, dr, staging), it requires key material to
be issued by CA company. Such key material persisted in Vault, so the
following template shows saving secure data in files.

\# webhooks external digicert certificate

template {

contents = \<\<EOH

{{ with secret "kv/data/btiq/btiq-gb03-staging-webhooks" -}}{{
.Data.data.digicertCertificate }}{{- end }}

EOH

destination =
"/vault/secrets/additionalCertificatesAndPrivateKeys/digicert.crt"

}

\# webhooks external certificate private key

template {

contents = \<\<EOH

{{ with secret "kv/data/btiq/btiq-gb03-staging-webhooks" -}}{{
.Data.data.digicertPrivateKey }}{{- end }}

EOH

destination =
"/vault/secrets/additionalCertificatesAndPrivateKeys/digicert.key"

}

...

\# this is the db.com root CA

template {

contents = \<\<EOH

{{ with secret "kv/data/btiq/btiq-gb03-staging-webhooks" -}}{{
.Data.data.deutscheBankGroupRootCa6CA }}{{- end }}

EOH

destination =
"/vault/secrets/additionalCAs/deutscheBankGroupRootCa6CA.crt"

}

\# this is the db.com intermediate CA

template {

contents = \<\<EOH

{{ with secret "kv/data/btiq/btiq-gb03-staging-webhooks" -}}{{
.Data.data.digicertEVRSACAG2 }}{{- end }}

EOH

destination = "/vault/secrets/additionalCAs/digicertEVRSACAG2.crt"

}

\# this is the db.com CA bundle for mtls

template {

contents = \<\<EOH

{{ with secret "kv/data/btiq/btiq-gb03-staging-webhooks" -}}{{
.Data.data.deutscheBankGlueXCABundle }}{{- end }}

EOH

destination =
"/vault/secrets/additionalCAs/deutscheBankGlueXCABundle.crt"

To tell the base image the location where to lookup for extra files,
Webhooks introduces environment variables in
chart/templates/deployment.yaml:

...

containers:

\- name: {{ .Chart.Name }}-runtime

image: "btiq/webhooks-runtime:{{ .Chart.Version }}"

imagePullPolicy: IfNotPresent

env:

...

\- name: "ADDITIONAL_KEYSTORE_ENTRIES_LOCATION"

value: {{ .Values.additionalKeystoreEntriesLocation \| quote }}

\- name: "EXTRA_SECRETS_DIRECTORY"

value: {{ .Values.additionalTruststoreEntriesLocation \| quote }}

In our case these locations are (values.yaml) :

additionalKeystoreEntriesLocation:
/vault/secrets/additionalCertificatesAndPrivateKeys

additionalTruststoreEntriesLocation: /vault/secrets/additionalCAs

# **Testing**

## **Unit Testing**

MTLSTest in Webhooks proves httpClient bean can connect with mTLS.

## **Integration Testing**

Webhooks uses Webhooks Testing Service (WTS) to test real-life scenarios
where WTS is a subscriber side.

Repo: <https://gitlab.saas-p.com/btiq/core-platform-services/services/webhooks-testing-service>

WTS as a service requires a client certificate, extraction from
application.properties:

server.ssl.client-auth=need

Invoking WTS through F5 does not allow to establish mTLS connection as
F5 does not pass the client certificate. To resolve it, WTS has been put
behind Gateway Sample Service (GSS). 

Similar to Webhooks, GSS establish mTLS connection using the same
paradigm as described above: Apache HTTP Client and importing secret
material from Vault to jks files.

[]{#Bookmark61 .anchor}**5 - Create subscriptions**

The Segment implementation team must set up the subscriptions on behalf
of the customer.

*       E.g. Payment confirmations for customer1 will be sent to
<https://customer1.com/payment-confirmed>*

-   [APIs to manage subscriptions](#apis-to-manage-subscriptions)

-   [Signing request data](#signing-request-data)

    -   [1. Shared Secret](#shared-secret)

    -   [2. Non-repudiation](#non-repudiation)

-   [Retry Process Notes](#retry-process-notes)

    -   [1. Default retry period](#default-retry-period)

    -   [2. Custom specified retry
        period](#custom-specified-retry-period)

# **APIs to manage subscriptions**

APIs to manage subscriptions are

+------+---------------------------+---------------------------------+
| **Ty | **API**                   | **Description**                 |
| pe** |                           |                                 |
+======+===========================+=================================+
| POST | /webhooks/subscriptions   | Create a subscription.          |
|      |                           | Attributes are                  |
|      |                           |                                 |
|      |                           | -   Customer end-point URL      |
|      |                           |                                 |
|      |                           | -   List of events (we can send |
|      |                           |     multiple events to the same |
|      |                           |     customer end-point)         |
|      |                           |                                 |
|      |                           | -   Product Id                  |
|      |                           |                                 |
|      |                           | -   (optional) Non-Repudiation  |
|      |                           |     Enabled (refer to note      |
|      |                           |     below for details on        |
|      |                           |     signing request data)       |
|      |                           |                                 |
|      |                           | And optionally (refer to note   |
|      |                           | below for more details on       |
|      |                           | automatic retries)              |
|      |                           |                                 |
|      |                           | -   first retry                 |
|      |                           |                                 |
|      |                           | -   retry exponent              |
|      |                           |                                 |
|      |                           | -   max retries                 |
|      |                           |                                 |
|      |                           | Note that you can define        |
|      |                           | multiple subscriptions related  |
|      |                           | to the same event (if the       |
|      |                           | customer wants us to call       |
|      |                           | multiple end-points when an     |
|      |                           | event is triggered)             |
+------+---------------------------+---------------------------------+
| PUT  | /webhooks/subscr          | Update an existing subscription |
|      | iptions/{subscription_id} |                                 |
+------+---------------------------+---------------------------------+
| DE   | /webhooks/subscr          | Delete an existing subscription |
| LETE | iptions/{subscription_id} |                                 |
+------+---------------------------+---------------------------------+
| GET  | /webhooks/subscr          | Get information on a given      |
|      | iptions/{subscription_id} | subscription. Returned          |
|      |                           | attributes are                  |
|      |                           |                                 |
|      |                           | -   Subscription Id             |
|      |                           |                                 |
|      |                           | -   Customer end-point URL      |
|      |                           |                                 |
|      |                           | -   Date created                |
|      |                           |                                 |
|      |                           | -   Date Updated                |
|      |                           |                                 |
|      |                           | -   List of events              |
|      |                           |                                 |
|      |                           | -   tenant Id                   |
|      |                           |                                 |
|      |                           | -   productId                   |
|      |                           |                                 |
|      |                           | -   subscription secret         |
|      |                           |     (encrypted in database) -   |
|      |                           |     only if non-repudiation is  |
|      |                           |     disabled                    |
|      |                           |                                 |
|      |                           | -   firstRetry                  |
|      |                           |                                 |
|      |                           | -   retryExponent               |
|      |                           |                                 |
|      |                           | -   maxRetries                  |
|      |                           |                                 |
|      |                           | -   isNonRepudiationEnabled     |
|      |                           |     (If "true", we will use a   |
|      |                           |     signing certificate. If     |
|      |                           |     "false", we will use a      |
|      |                           |     shared secret.)             |
|      |                           |                                 |
|      |                           | -   key id to identify the      |
|      |                           |     public key - only if        |
|      |                           |     non-repudiation is enabled  |
+------+---------------------------+---------------------------------+
| GET  | /webhooks/subscriptions   | Gets all subscriptions for a    |
|      |                           | tenant. Returned a list of      |
|      |                           | objects with the same           |
|      |                           | attributes as above             |
+------+---------------------------+---------------------------------+

# **Signing request data**

Webhooks can sign the payload using one of these two approaches:

1.  Shared Secret  (default approach and less secure)

2.  Non-repudiation (more secure)

Non-repudiation needs to be specifically enabled for a subscription
(default being shared secret)

## **1. Shared Secret**

Shared Secret as a feature is implemented through the [sync
keys]{.underline} paradigm.

The shared secret is a unique UUID provided when creating a
subscription. 

Request:

POST
https://btiq-dynamic.saas-n.com/btiq-5231/api-gateway-private/webhooks/1.0/webhooks/subscriptions

{

"url":
"https://btiq-sample-stable.saas-n.com/btiq-webhooks-testing-service/webhooks",

"events": \[

"GoodThingHappenned"

\],

"productId": "{{productId1}}"

}

------------------------------------------------------------------------

Response:

{

"id": "631fa62e-5456-44e6-8a71-962877d98ac1",

"url":
"https://btiq-sample-stable.saas-n.com/btiq-webhooks-testing-service/webhooks",

"created": "2022-08-29T15:25:55.291611",

"updated": "2022-08-29T15:25:55.291611",

"events": \[

"GoodThingHappenned"

\],

"tenantId": "e2293816-8886-4656-8db4-35ed67f2ac31",

"productId": "bf130a3f-3cc6-4120-9975-2279042c51d7",

"secret": "9c2b4d03-a68c-4878-8a99-939676edbdaf",

"firstRetry": 30.0,

"retryExponent": 4,

"maxRetries": 4,

"isNonRepudiationEnabled": false

}

As you can see from the response above, by default non-repudiation is
false and generated shared secret is
"9c2b4d03-a68c-4878-8a99-939676edbdaf"

**The shared secret must be communicated to the customer (in a secure
way)**

## **2. Non-repudiation**

Non-repudiation as a feature is implemented through the [async
keys]{.underline} paradigm. The non-repudiation certificate is managed
by the Shared Platform team. There is only one certificate that all
customers use.

**INSERT CERTIFICATE AND EXPIRY DATE HERE**

**The non-repudiation certificate must be communicated to the customer**
(no need for secured communication as this is a public key)

# **Retry Process Notes**

Webhooks follow an exponential backoff algorithm to implement automatic
retries through the outbox library.

The following parameters with default values are configurable and used
to achieve the exponential backoff:

-  firstRetry \
-  retryExponent \
-  maxRetries 

**[Retry parameters shall be set according to the customer's
SLA]{.underline}**

## **1. Default retry period**

If retry values are not specified during subscription, default values
are:

-  firstRetry : 30.0 sec\
-  retryExponent : 4\
-  maxRetries : 4

Using the above parameters, the algorithm is defined as ***failCount
==1? firstRetry: firstRetry\*Math.pow(failCount,retryExponent)***

-   when failCount ==1 then the first retryPeriod happens at **30 sec**

-   when failCount ==2 then the next retryPeriod happens at 30\*(2,4)
    = **8 min**

-   when failCount ==3 then the next retryPeriod happens at 30\*(3,4)
    = **40 min 30 sec**

-   when failCount ==4 then the next retryPeriod haooens at  30\*(4,4)
    = **2h 8min**

**30s, 8m, 40m 30s, 2h 8m** are the default retry periods.

## **2. Custom specified retry period**

Customers can create webhooks subscriptions for the specified retries.

-   retryExponent can't be less than 2

-   firstRetry must be \>=1sec and should be given in sec

-   maxRetries = 0 means , never automatically retry

-   maxRetries \< = 5

-   total retryPeriod shouldn't exceed 24h.

**Examples of Requests and Responses**

Please note that you need encryption keys before testing the requests
below.

### Subscription

This is an object representing your Webhooks subscription.

  **Endpoints**
  ------------------------------------
  /webhooks/1/webhooks/subscriptions

All operations require the following HTTP header to be present:
X-On-Behalf-Of

### The subscription object.

#### Attributes

+---------------+----------------+------+---------------------------+
| id            | Subscription   | UUID | Read-only attribute,      |
|               | id             | type | shouldn't be used in      |
|               |                |      | creating or updating      |
|               |                |      | operations                |
+===============+================+======+===========================+
| URL           | URL of         | URL  | Mandatory attribute       |
|               | webhooks       | t    |                           |
|               | target request | ype  |                           |
+---------------+----------------+------+---------------------------+
| created       | Date when the  | Date | Read-only attribute,      |
|               | object was     | t    | shouldn't be used in      |
|               | created        | ype  | creating or updating      |
|               |                |      | operations                |
+---------------+----------------+------+---------------------------+
| updated       | Date when the  | Date | Read-only attribute,      |
|               | object was     | type | shouldn't be used in      |
|               | updated        |      | creating or updating      |
|               |                |      | operations                |
+---------------+----------------+------+---------------------------+
| events        | Type(s) of     | A    | Mandatory attribute       |
|               | events that    | rray |                           |
|               | can be         | of   |                           |
|               | processed      | st   |                           |
|               |                | ring |                           |
|               |                | type |                           |
+---------------+----------------+------+---------------------------+
| tenantId      | ID of a tenant | UUID | Read-only attribute,      |
|               |                | type | shouldn't be used in      |
|               |                |      | creating or updating      |
|               |                |      | operations                |
+---------------+----------------+------+---------------------------+
| productId     | ID of a        | UUID | Mandatory attribute for   |
|               | product        | type | creation cannot be        |
|               |                |      | updated                   |
+---------------+----------------+------+---------------------------+
| secret        | Used for       | UUID | Read-only attribute,      |
|               | validating     | type | shouldn't be used in      |
|               | webhooks       |      | creating or updating      |
|               | requests on    |      | operations                |
|               | the receiving  |      |                           |
|               | side           |      |                           |
+---------------+----------------+------+---------------------------+
| firstRetry    | Value for      | F    |                           |
|               | automatic      | loat |                           |
|               | retrying of    | type |                           |
|               | failed         |      |                           |
|               | requests       |      |                           |
|               | (seconds)\     |      |                           |
|               | Default value  |      |                           |
|               | is 30 seconds. |      |                           |
+---------------+----------------+------+---------------------------+
| retryExponent | Value for      | Int  |                           |
|               | calculating    | eger |                           |
|               | exponential    | type |                           |
|               | timeouts for   |      |                           |
|               | automatic      |      |                           |
|               | retries.\      |      |                           |
|               | Default value  |      |                           |
|               | is 4           |      |                           |
+---------------+----------------+------+---------------------------+
| maxRetries    | Number of      | Int  |                           |
|               | maximum        | eger |                           |
|               | automatic      | type |                           |
|               | retries        |      |                           |
|               |                |      |                           |
|               | Default value  |      |                           |
|               | is 4           |      |                           |
+---------------+----------------+------+---------------------------+
| isNonRepud    | Marks          | Boo  |                           |
| iationEnabled | subscription   | lean |                           |
|               | as using       | type |                           |
|               | n              |      |                           |
|               | on-repudiation |      |                           |
|               | type of        |      |                           |
|               | c              |      |                           |
|               | ommunication.\ |      |                           |
|               | Default value  |      |                           |
|               | is false       |      |                           |
+---------------+----------------+------+---------------------------+
| kid           | Key ID value   | UUID | Read-only attribute,      |
|               |                | type | shouldn't be used in      |
|               |                |      | creating or updating      |
|               |                |      | operations. Used only if  |
|               |                |      | 'isNonRepudiationEnabled' |
|               |                |      | is set to true.           |
+---------------+----------------+------+---------------------------+

The subscription object.

{

"id": "ea6039ae-b83f-4a10-a25d-7d1b0f412a17",

"url": "https://example.com",

"created": "2022-08-29T11:54:48.889545",

"updated": "2022-08-29T11:54:48.889545",

"events": \[

"TEST_EVENT",

"TEST_PAYMENT"

\],

"tenantId": "a138f0fb-7df8-4b77-9632-741b71e4b2fc",

"productId": "a2996b42-2bb8-4ad5-91d2-bb6a1c5a577d",

"secret": "805f7ce0-4c6b-426c-8f03-ee2ea95b1996",

"firstRetry": 30.0,

"retryExponent": 4,

"maxRetries": 4,

"isNonRepudiationEnabled": false

}

### List Subscriptions

**Parameters (set in the URL)**

+------+----------------------------+--------------------------------+
| Page | Starting page.             | Default value is 0.            |
+======+============================+================================+
| Size | Number of elements on a    | Default value is 50.           |
|      | page                       |                                |
+------+----------------------------+--------------------------------+
| Sort | Sorting parameters         | Default sorting is by          |
|      |                            |                                |
|      |                            | 'updated' field in descending  |
|      |                            | order.                         |
+------+----------------------------+--------------------------------+

#### Request Headers

X-On-Behalf-Of {tenantId}

**Returns**

List of subscriptions.

**Endpoint**

  -----------------------------------------------------------------------
  GET /webhooks/1/webhooks/subscriptions?page=0&size=50&sort=updated,desc
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

**Response**

{

"\_embedded": {

"subscriptions": \[

{

"id": "bbd7bee4-8e97-4751-80fe-3faf1a5cb2c2",

"url":
"https://webhooks-testing-service-unstable.btiq-ny2-integration-webhooks.svc.cluster.local:8543/webhooks",

"created": "2022-08-30T20:17:37.649101",

"updated": "2022-08-30T20:17:37.649101",

"events": \[

"bxBqxMu"

\],

"tenantId": "a138f0fb-7df8-4b77-9632-741b71e4b2fc",

"productId": "026102b0-4cc1-462d-bd8f-682818db0639"

},

{

"id": "0d7e27a2-9961-4539-a624-d5988cc1d570",

"url":
"https://webhooks-testing-service-unstable.btiq-ny2-integration-webhooks.svc.cluster.local:8543/webhooks",

"created": "2022-08-30T20:17:33.071655",

"updated": "2022-08-30T20:17:33.071655",

"events": \[

"bxBqxMu"

\],

"tenantId": "a138f0fb-7df8-4b77-9632-741b71e4b2fc",

"productId": "026102b0-4cc1-462d-bd8f-682818db0639"

}

\]

},

"\_links": {

"first": {

"href":
"https://btiq-unstable.saas-n.com/webhooks/webhooks/subscriptions?page=0&size=2&sort=updated,desc",

"title": ""

},

"self": {

"href":
"https://btiq-unstable.saas-n.com/webhooks/webhooks/subscriptions?page=0&size=2&sort=updated,desc",

"title": ""

},

"next": {

"href":
"https://btiq-unstable.saas-n.com/webhooks/webhooks/subscriptions?page=1&size=2&sort=updated,desc",

"title": ""

},

"last": {

"href":
"https://btiq-unstable.saas-n.com/webhooks/webhooks/subscriptions?page=7868&size=2&sort=updated,desc",

"title": ""

}

},

"page": {

"size": 2,

"totalElements": 15737,

"totalPages": 7869,

"number": 0

}

}

### Create Subscription

**Parameters**

1.  The subscription object in the body without read-only fields.

2.  {

3.  "url": "https://google.com",

4.  "events": \["TEST_GOOGLE"\],

5.  "productId": "a2996b42-2bb8-4ad5-91d2-bb6a1c5a577d"

> }

#### Request Headers

> X-On-Behalf-Of {tenantId}
>
> 'X-Idempotency-Key' HTTP header in UUID format to ensure the
> idempotency of an operation.

-   This is a required parameter for this particular request. Details
    specific to webhooks use of the idempotency library are included
    [here](http://Miscellaneous)

**Returns**

Created subscription with all fields populated.

**Endpoint**

  POST /webhooks/1/webhooks/subscriptions
  -----------------------------------------

**Response**

{

"id": "66921a3c-bbb7-4223-85c9-58ab6325b02f",

"url": "https://google.com",

"created": "2022-08-30T16:15:42.522043",

"updated": "2022-08-30T16:15:42.522043",

"events": \[

"TEST_ERROR"

\],

"tenantId": "a138f0fb-7df8-4b77-9632-741b71e4b2fc",

"productId": "a2996b42-2bb8-4ad5-91d2-bb6a1c5a577d",

"secret": "cdf4008b-9d0c-4277-8a20-4312ac495b5a",

"firstRetry": 30.0,

"retryExponent": 4,

"maxRetries": 4,

"isNonRepudiationEnabled": false

}

### Get Subscription

**Parameters**

The subscription ID in the URL.

#### Request Headers

X-On-Behalf-Of {tenantId}

**Returns**

Subscription object with all fields populated.

**Endpoint**

  GET /webhooks/1/webhooks/subscriptions/{subscription_id}
  ----------------------------------------------------------

**Response**

{

"id": "66921a3c-bbb7-4223-85c9-58ab6325b02f",

"url": "https://google.com",

"created": "2022-08-30T16:15:42.522043",

"updated": "2022-08-30T16:15:42.522043",

"events": \[

"TEST_ERROR"

\],

"tenantId": "a138f0fb-7df8-4b77-9632-741b71e4b2fc",

"productId": "a2996b42-2bb8-4ad5-91d2-bb6a1c5a577d",

"secret": "cdf4008b-9d0c-4277-8a20-4312ac495b5a",

"firstRetry": 30.0,

"retryExponent": 4,

"maxRetries": 4,

"isNonRepudiationEnabled": true,

"kid": "b0dd9114-e3c6-457a-bd28-dc991fa27200"

}

### Update Subscription

**Parameters**

The subscription object in the body.

{

"url": "https://google.com",

"events": \["TEST_GOOGLE"\],

"productId": "a2996b42-2bb8-4ad5-91d2-bb6a1c5a577d",

"isNonRepudiationEnabled": true

}

**Returns**

Updated subscription with all fields populated.

**Endpoint**

  PUT /webhooks/1/webhooks/subscriptions/{subscription_id}
  ----------------------------------------------------------

**Response**

{

"id": "66921a3c-bbb7-4223-85c9-58ab6325b02f",

"url": "https://google.com",

"created": "2022-08-30T16:15:42.522043",

"updated": "2022-08-30T16:15:42.522043",

"events": \[

"TEST_ERROR"

\],

"tenantId": "a138f0fb-7df8-4b77-9632-741b71e4b2fc",

"productId": "a2996b42-2bb8-4ad5-91d2-bb6a1c5a577d",

"secret": "cdf4008b-9d0c-4277-8a20-4312ac495b5a",

"firstRetry": 30.0,

"retryExponent": 4,

"maxRetries": 4,

"isNonRepudiationEnabled": true,

"kid": "b0dd9114-e3c6-457a-bd28-dc991fa27200"

}

### Delete Subscription

**Parameters**

The subscription id in the URL.

**Returns**

Empty body.

**Endpoint**

  DELETE /webhooks/1/webhooks/subscriptions/{subscription_id}
  -------------------------------------------------------------

**Response**

Empty body.

[]{#Bookmark81 .anchor}**6 - Tests**

# **Challenge Response Checks**

Challenge-Response Check (CRC) is implemented in webhooks to verify a
subscriber is the owner of the webhooks URL

  ----------------------------------------------------------------------------------------------------------------
  **Type**   **API**                                                **Description**
  ---------- ------------------------------------------------------ ----------------------------------------------
  POST       /webhooks/subscriptions/{subscription_id}/crc          Calls the customer end-point URL to verify he
                                                                    is effectively the owner of the end-point URL
                                                                    defined in the subscription

  GET        /webhooks/subscriptions/{subscription_id}/crc/status   Returns the status of the latest successful
                                                                    and failed CRC event

  GET        /webhooks/subscriptions/{subscription_id}/crc           Returns the history of all CRC events
                                                                    presented per page
  ----------------------------------------------------------------------------------------------------------------

# **Bottomline Testing Control Process**

Bottomline expects customers to test on a UAT environment before being
promoted to production. Below is a set of criteria that we expect all
clients to adhere to and comprehensively test within the UAT environment
before confirming the completion of testing within the UAT sign-off
document.

**To be defined, but good basis from FM here.**

![](./media/image24.png){style="width:2.60417in;height:2.60417in"}

**6.1 Examples of Requests and Responses**

-   [Triggering CRC event](#triggering-crc-event)

-   [Check CRC status](#check-crc-status)

-   [View CRC history](#view-crc-history)

# Triggering CRC event

When CRC is invoked in Webhooks, it makes a GET request to the
subscriber URL with "crc" parameter and random UUID as the value. When
that request is received, the webhooks subscriber needs to build an
encrypted response token based on the "crc" parameter and the
subscriber's secret key got upon subscription. It should make a
signature based on crc UUID in the parameter and send such a signature
back.

Webhooks get a signature from the response and validate it based on
shared secret and crc UUID value.

CRC may work only for subscriptions with disabled non-repudiation. 

**Parameters**

The subscription id in the URL.

**Returns**

Empty body.

**Endpoint**

  POST /webhooks/1/webhooks/subscriptions/{subscription_id}/crc
  ---------------------------------------------------------------

**Response**

Empty body.

Webhooks save the result of every triggering CRC event. 

If subscriber's response to the GET request is an error (4xx or 5xx),
then webhooks save it with the status FAILURE and the failure type is
4xx or 5xx code. Otherwise, it gets a signature and validates. If the
signature is invalid, it saves the result with status FAILURE and
failure type 400. If valid, then it saves as SUCCESS.

Note: Webhooks uses outbox lib to guarantee sending GET requests to
subscribers.

# Check CRC status

When a CRC event is triggered, it makes sense to check its status.

Webhooks have the ability to represent data for the latest successful
and failed CRC event:

**Parameters**

1.  The subscription id in the URL.

**Returns**

CRC status data.

**Endpoint**

  GET /webhooks/1/webhooks/subscriptions/{subscription_id}/crc/status
  ---------------------------------------------------------------------

**Response**

{

"subscriptionId": "377c1e29-e0c6-49ef-988d-d9e72238b8a2",

"failureType": "405",

"successDate": 2022-09-19T12:23:15.777684,

"failureDate": "2022-09-05T12:23:15.777684"

}

The response above indicates that the last CRC was done on 2022-09-19,
and it was successful. The latest unsuccessful attempt was made on
2022-09-05, it failed with 405 - method is not allowed.

# View CRC history

Webhooks allow seeing the history of all CRC events presented per page
(the default quantity of entities on the page is 50 with the first page
by default and sorted by date in descending order by default). 

It is also possible to sort by status and have ascending order. The
current page and page size also can be set in the URL.

**Parameters (set in the URL)**

+------+-----------------------------+------------------------------+
| Page | Starting page.              | The default value is 0.      |
+======+=============================+==============================+
| Size | Number of elements on a     | The default value is 50.     |
|      | page                        |                              |
+------+-----------------------------+------------------------------+
| Sort | Sorting parameters          | Default sorting is by        |
|      |                             |                              |
|      |                             | 'date' field in descending   |
|      |                             | order.                       |
+------+-----------------------------+------------------------------+

**Returns**

CRC history for a subscription.

**Endpoint**

  -------------------------------------------------------------------------------------------
  GET
  /webhooks/1/webhooks/subscriptions/{subscription_id}/crc?page=0&size=50&sort=updated,desc
  -------------------------------------------------------------------------------------------

  -------------------------------------------------------------------------------------------

**Response**

{

"\_embedded": {

"crcHistory": \[

{

"id": "6b8bc9d8-ef91-45b6-98d9-d608418f7841",

"status": "FAILURE",

"failureType": "405",

"date": "2022-09-05T12:23:46.843899"

},

{

"id": "1e0e37af-ad07-46b5-a1ce-e4db1973a50a",

"status": "FAILURE",

"failureType": "405",

"date": "2022-09-05T12:23:15.777684"

}

\]

},

"\_links": {

"self": {

"href":
"https://btiq-unstable.saas-n.com/webhooks/webhooks/subscriptions/377c1e29-e0c6-49ef-988d-d9e72238b8a2/crc?page=0&size=50&sort=date,desc",

"title": ""

}

},

"page": {

"size": 50,

"totalElements": 2,

"totalPages": 1,

"number": 0

}

}

[]{#Bookmark88 .anchor}**Non Repudiation Certificates**

This page contains public certificates required for non-repudiation.
These certificates are valid for a year from the date of creation and
will be updated regularly as they approach their end of life.

  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Environment**   **Non Repudiation Public Certificate**                                                                                                                                      **Valid    **Valid   **Last
                                                                                                                                                                                                Until**    For**     Updated**
  ----------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------- --------- -----------
  SAAS-N Stable     [stable.crt](https://confluence.bottomline.tech/download/attachments/376874491/webhooks-jwt-stable_bottomline_com.crt?version=1&modificationDate=1675759513000&api=v2)      Jan 19     1 Year    Feb 7, 2023
                                                                                                                                                                                                23:59:59             
                                                                                                                                                                                                2024 GMT             

  UK Staging        [uk                                                                                                                                                                         Jan 20     1 Year    Feb 7, 2023
                    staging.crt](https://confluence.bottomline.tech/download/attachments/376874491/webhooks-jwt-staging_bottomline_co_uk.crt?version=1&modificationDate=1675759403000&api=v2)   23:59:59             
                                                                                                                                                                                                2024 GMT             

  US Staging        [us staging.crt](https://confluence.bottomline.tech/download/attachments/376874491/webhooks-jwt-staging_bottomline_com.crt?version=1&modificationDate=1675759397000&api=v2) Jan 19     1 Year    Feb 7, 2023
                                                                                                                                                                                                23:59:59             
                                                                                                                                                                                                2024 GMT             

  CH Staging        [ch staging.crt](https://confluence.bottomline.tech/download/attachments/376874491/webhooks-jwt-staging_bottomline_ch.crt?version=1&modificationDate=1675759409000&api=v2)  Jan 30     1 Year    Feb 7, 2023
                                                                                                                                                                                                23:59:59             
                                                                                                                                                                                                2024 GMT             

  UK Production     [uk prod.crt](https://confluence.bottomline.tech/download/attachments/376874491/gb00%20prod.crt?version=1&modificationDate=1672751587000&api=v2)                            Dec 19     1 Year    NA
                                                                                                                                                                                                23:59:59             
                                                                                                                                                                                                2023 GMT             

  US Production     [us prod.crt](https://confluence.bottomline.tech/download/attachments/376874491/webhooks-jwt-pr1_bottomline_com.crt?version=1&modificationDate=1676013020000&api=v2)        Jan 19     1 Year    Feb 10,
                                                                                                                                                                                                23:59:59             2023
                                                                                                                                                                                                2024 GMT             

  CH production     [ch prod.crt](https://confluence.bottomline.tech/download/attachments/376874491/webhooks-jwt-pr1_bottomline_ch.crt?version=1&modificationDate=1676013007000&api=v2)         Jan 30     1 Year    Feb 10,
                                                                                                                                                                                                23:59:59             2023
                                                                                                                                                                                                2024 GMT             
  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Implementation Guide**

**This section is intended for the Customer's development team**

A webhook is an HTTP callback: an HTTP POST that occurs when something
happens - an event notification via HTTP POST. Webhooks are used for
real-time notifications, so your system can be updated right when the
event takes place. 

Whenever you subscribe to a Bottomline solution that proposes webhooks,
you need to follow below process:

![](./media/image25.png){style="width:4.875in;height:1.05556in"}

+---+---------+---------------------------------------------------------+
| 1 | [Im     | Implements end-point(s) that will receive and manage    |
|   | plement | notifications.                                          |
|   | Webhook |                                                         |
|   | end     | *   E.g. <https://customer1.com/payment-confirmed> will |
|   | -points | manage payment confirmations*                           |
|   | ](#Book |                                                         |
|   | mark90) | You need to send your endpoint URLs to Bottomline, so   |
|   |         | those can be whitelisted.                               |
+===+=========+=========================================================+
| 2 | [Setup  | mTLS require certificates to be generated on both sides |
|   | mTLS    | (Customer and Bottomline). Certificates need to be      |
|   | (op     | stored/secured on both sides                            |
|   | tional) |                                                         |
|   | ](#Book |                                                         |
|   | mark97) |                                                         |
+---+---------+---------------------------------------------------------+
| 3 | [Store  | Bottomline will create subscriptions on your behalf     |
|   | /secure |                                                         |
|   | shared  |      *E.g. Payment confirmations will be sent           |
|   | secret  | to* <https://customer1.com/payment-confirmed>           |
|   | or      |                                                         |
|   | cert    | You can choose between shared secret and non            |
|   | ificate | repudiation. Depending on your choice, Bottomline will  |
|   | ](#Book | send you a shared secret or a signing certificate       |
|   | mark98) | related to each subscription. You need to               |
|   |         | store/secure the shared secret or the signing           |
|   |         | certificate related to your subscriptions               |
+---+---------+---------------------------------------------------------+
| 4 | [Tests  | When you are ready to test, Bottomline will trigger a   |
|   | ](#Book | CRC (Challenge Response Check) process                  |
|   | mark99) |                                                         |
+---+---------+---------------------------------------------------------+

[]{#Bookmark90 .anchor}**1 - Implement Webhook end-points**

**This section is intended for the Customer's development team**

Payload must not be altered

The customer-side payload must be validated as it is (unchanged string).

Any deserializations to the business object and back to the string may
alter the initial payload, so the signature becomes invalid.

-   [Code snippet of shared secret
    validation](#code-snippet-of-shared-secret-validation)

-   [Code snippet of non-repudiation
    validation](#code-snippet-of-non-repudiation-validation)

    -   [Step 1. Obtaining signing string from JWT and validating with
        public
        key](#step-1.-obtaining-signing-string-from-jwt-and-validating-with-public-key)

    -   [Step 2. Calculating signing string based on request
        data](#step-2.-calculating-signing-string-based-on-request-data)

    -   [Step 3. Comparing provided and calculated signing
        strings](#step-3.-comparing-provided-and-calculated-signing-strings)

-   [Code snippet to create a signature by subscriber per CRC
    call](#code-snippet-to-create-a-signature-by-subscriber-per-crc-call)

# **Code snippet of shared secret validation**

/\*\*

\* checks if a given timestamp is in a range of acceptable window of
time

\*

\* [\@param]{.citation cites="param"} timestamp the timestamp to check

\* [\@return]{.citation cites="return"} a boolean indicating if the
timestamp falls in the window or not

\*/

private boolean isValidTimestamp(long timestamp) {

long now =
LocalDateTime.now(ZoneOffset.UTC).toInstant(ZoneOffset.UTC).getEpochSecond();

long window = TimeUnit.MINUTES.toSeconds(TIMESTAMP_WINDOW_MINUTES);

long lowThreshold = timestamp - window;

long highThreshold = timestamp + window;

boolean validTimestamp = now \>= lowThreshold && now \<= highThreshold;

log.info("Timestamp received {} threshold {} threshold {}", now,
lowThreshold, highThreshold);

log.info("Timestamp validated {}", validTimestamp ? "successfully" :
"unsuccessfully");

return validTimestamp;

}

/\*\*

\* checks if jws is valid given the received payload and shared secret

\*

\* [\@param]{.citation cites="param"} jws - the signature in jws format

\* [\@param]{.citation cites="param"} payload - the received payload to
use to check the jws

\* [\@return]{.citation cites="return"} boolean indicating if the
signature is valid or not

\*/

private boolean isValidSignature(String jws, String payload) {

try {

JsonWebSignature verifierJws = new JsonWebSignature();

Key key = new HmacKey(secretKey.getBytes(StandardCharsets.UTF_8));

verifierJws.setCompactSerialization(jws);

verifierJws.setPayload(payload);

verifierJws.setKnownCriticalHeaders(JWS_HEADER_CUSTOM_IAT);

verifierJws.setKey(key);

long timestamp =
verifierJws.getHeaders().getLongHeaderValue(JWS_HEADER_CUSTOM_IAT);

if (!isValidTimestamp(timestamp)) return false;

return verifierJws.verifySignature();

} catch (JoseException e) {

log.error(e.getMessage());

return false;

}

}

# **Code snippet of non-repudiation validation**

Validating of request includes three main steps:

1.  Obtaining the signing string from JWT and validating with a public
    key

2.  Calculating signing string based on request data

3.  Comparing provided signing string from step 1 and the calculated
    signing string from step 2

## **Step 1.  Obtaining signing string from JWT and validating with public key**

JWT consists of three parts:

-   header

-   payload

-   signature

For more information about JWT refer:
[jwt.io](https://jwt.io/introduction)

In our case that is how the structure of the JWT payload should look
like:

{

"iat": 1643992393,

"digest": {

"alg": "SHA256",

"headerList": "x-idempotency-key",

"value":
"8b07cf5f3d5183e5265dbde6319487b0f80733a3fc08adaa7217657b3693eefa"

}

}

Basically in the code below, we validate the signature and obtain the
signing string value from the "value" attribute of JSON.

public static String getSigningString(String jwt, PublicKey publicKey)
throws JwtValidationException {

String signingString;

try {

JwtConsumer jwtConsumer = new JwtConsumerBuilder()

.setVerificationKey(publicKey) // verify the signature with the public
key

.build(); // create the JwtConsumer instance

final Object digest =
jwtConsumer.processToClaims(jwt).getClaimValue("digest");

if (!(digest instanceof Map)) {

throw new JwtValidationException("Invalid digest claim:" + digest);

}

signingString = ((Map\<String, String\>) digest).get("value");

} catch (InvalidJwtException e) {

throw new JwtValidationException("Can't get signing string from jwt",
e);

}

if (StringUtils.isBlank(signingString)) {

throw new JwtValidationException("Signing String can't be blank");

}

return signingString;

}

**Provided signing string
is: 8b07cf5f3d5183e5265dbde6319487b0f80733a3fc08adaa7217657b3693eefa**

## **Step 2. Calculating signing string based on request data**

To calculate the signing string, do the following :

-   collect the required data from the request

-   create raw signing string based on such data

-   encode raw signing string

**Collect required data**

public static String calculateSigningString(String httpMethod, String
jwt, HttpServletRequest request,

String payload) {

final Map\<String, String\> headersMap = new HashMap\<\>();

final String body = new
String(Base64.decodeBase64(jwt.split("\\.")\[1\]));

final String headers =
JsonPath.parse(body).read("\$.digest.headerList");

if (StringUtils.isNotBlank(headers)) {

final String\[\] headersArr = headers.split(";");

for (String header : headersArr) {

final String headerValue = request.getHeader(header);

headersMap.put(header, headerValue);

}

}

final String url = request.getScheme() + "://" +

request.getServerName() +

("http".equals(request.getScheme()) && request.getServerPort() == 80
\|\| "https".equals(request.getScheme()) && request.getServerPort() ==
443 ? "" : ":" + request.getServerPort()) +

request.getRequestURI() +

(request.getQueryString() != null ? "?" + request.getQueryString() :
"");

final String signingString =
encodeString(createSigningString(httpMethod.toUpperCase(), url,
headersMap, payload));

return signingString;

}

In the code above, the system collects the data required for signing
string creation, then invokes a method which creates a raw signing
string (method *createSigningString*) and then invokes the method to
encode the signing string (method *encodeString*).

**Create raw signing string**

public static String createSigningString(String httpMethod, String url,
Map\<String, String\> headers, String payload) {

final UriComponentsBuilder uriComponentsBuilder =
UriComponentsBuilder.fromUriString(url);

final MultiValueMap\<String, String\> queryParams =
uriComponentsBuilder.build().getQueryParams();

String uri = uriComponentsBuilder.replaceQuery(null).toUriString();

String canonicalQueryString = getCanonicalQueryString(queryParams);

String canonicalHeadersString = getCanonicalHeaderString(headers);

String hexPayload = encodeString(payload);

String signingString = httpMethod + "" + uri + "" +
canonicalQueryString + "" + canonicalHeadersString +

"" + hexPayload;

return signingString;

}

public static String getCanonicalQueryString(Map\<String,
List\<String\>\> parameters) {

final SortedMap\<String, List\<String\>\> sorted = new TreeMap\<\>();

for (Map.Entry\<String, List\<String\>\> entry : parameters.entrySet())
{

final String paramName = entry.getKey();

final List\<String\> paramValues = entry.getValue();

final List\<String\> values = new ArrayList\<\>(

paramValues.size());

values.addAll(paramValues);

Collections.sort(values);

sorted.put(paramName, values);

}

final StringBuilder result = new StringBuilder();

for (Map.Entry\<String, List\<String\>\> entry : sorted.entrySet()) {

for (String value : entry.getValue()) {

if (result.length() \> 0) {

result.append("&");

}

result.append(entry.getKey()).append("=").append(value);

}

}

return result.toString();

}

private static String createHeaderString(Map\<String, String\> headers,
BiFunction\<String, StringBuilder, Void\> fn) {

final List\<String\> sortedHeaders = new ArrayList(headers.keySet());

sortedHeaders.sort(String.CASE_INSENSITIVE_ORDER);

StringBuilder buffer = new StringBuilder();

for (String header : sortedHeaders) {

fn.apply(header, buffer);

}

return buffer.toString();

}

public static String getCanonicalHeaderString(Map\<String, String\>
headers) {

BiFunction\<String, StringBuilder, Void\> fn =

(header, buffer) -\> {

String value = headers.get(header);

buffer.append(header.toLowerCase().trim());

buffer.append(":");

if (value != null) {

buffer.append(value.trim());

}

buffer.append("");

return null;

};

return createHeaderString(headers, fn);

}

The result raw signing string looks like:

POST

http://localhost/webhooks/non-repudiation

x-idempotency-key:0596ba8f-e875-4bcd-bf68-b84bc977091e

04c453367fc035387e572627c7446e215aaf972826b8a04c8d3bb2ec1aa7c134

The payload in the raw signing string is encoded the same way as the
signing string is encoded.

**Encode signing string**

Encoding signing string uses the same method as encoding payload:
encodeString (String rawString)

public static String encodeString(String rawString) {

final MessageDigest digest;

try {

digest = MessageDigest.getInstance("SHA-256");

} catch (NoSuchAlgorithmException e) {

LOGGER.error("Can't encode string", e);

return null;

}

final byte\[\] hashbytes = digest.digest(

rawString.getBytes(StandardCharsets.UTF_8));

return bytesToHex(hashbytes);

}

public static String bytesToHex(byte\[\] hash) {

StringBuilder hexString = new StringBuilder(2 \* hash.length);

for (byte b : hash) {

String hex = Integer.toHexString(0xff & b);

if (hex.length() == 1) {

hexString.append('0');

}

hexString.append(hex);

}

return hexString.toString();

}

Based on that raw signing string after encoding should look like that

**Calculated signing
string: 8b07cf5f3d5183e5265dbde6319487b0f80733a3fc08adaa7217657b3693eefa**

## **Step 3. Comparing provided and calculated signing strings **

providedSigningString.equals(calculatedSigningString)

The calculated signing strings are equal in the sample provided above,
so it is a valid request.

# **Code snippet to create a signature by subscriber per CRC call**

/\*\*

\* Creates a JWS (signature) given the payload and the shared secret

\*

\* [\@param]{.citation cites="param"} payload - the json string payload
to sign

\* [\@param]{.citation cites="param"} iat - the timestamp to use to sign

\* [\@param]{.citation cites="param"} sharedSecret - the shared secret
used for signing

\* [\@return]{.citation cites="return"} the JWS signature

\*/

static String createJWS(String payload, String sharedSecret) {

   long iat =
LocalDateTime.now(ZoneOffset.UTC).toInstant(ZoneOffset.UTC).getEpochSecond();

Key key = new HmacKey(sharedSecret.getBytes(StandardCharsets.UTF_8));

JsonWebSignature signerJws = new JsonWebSignature();

signerJws.setPayload(payload);

signerJws.setAlgorithmHeaderValue(AlgorithmIdentifiers.HMAC_SHA256);

signerJws.setKey(key);

signerJws.getHeaders().setObjectHeaderValue(HeaderParameterNames.BASE64URL_ENCODE_PAYLOAD,
false);

signerJws.getHeaders().setObjectHeaderValue(JWS_HEADER_CUSTOM_IAT, iat);

signerJws.setCriticalHeaderNames(

HeaderParameterNames.BASE64URL_ENCODE_PAYLOAD,

JWS_HEADER_CUSTOM_IAT

);

String detachedContentJws;

try {

detachedContentJws = signerJws.getDetachedContentCompactSerialization();

} catch (JoseException e) {

throw new ResponseStatusException(INTERNAL_SERVER_ERROR,
e.getMessage());

}

return detachedContentJws;

}

As payload webhooks send random UUID.  A shared secret is known per
subscription.

Returned signature is JsonWebSignature with excluded payload.

[]{#Bookmark97 .anchor}**2 - Setup MTLS (optional)**

Mutual TLS, or mTLS for short, is a method for mutual authentication.
mTLS ensures that the parties at each end of a network connection are
who they claim to be by verifying that they both have the correct
private key.

The information within their respective TLS certificates provides
additional verification.

mTLS is often used in a Zero Trust security framework\* to verify an
organisation's users, devices, and servers. It can also help keep APIs
secure.

\*Zero Trust means that no user, device, or network traffic is trusted
by default, an approach that helps eliminate many security
vulnerabilities

Webhooks uses Mutual TLS (mTLS) protocol on httpconnection between
itself and subscriber. Basically connection between webhooks and URL
host from the subscription should support mTLS.

As an HTTP connector with mTLS it uses Apache HTTP Client.

For mTLS, webhooks uses the certs issued by DigiCert. As Digicert is
trusted across the globe by all the HTTP clients,, therefore customers
do not have to take any specific action. For Webhooks to trust the
customer certs, the Customer can share the certificate chain used by
them during mTLS communication, the same can imported into Java trust
store, to ensure there is no issue during communication. Please put the
certificate chain in the on-boarding ticket and same can be imported
into Vault. 

[]{#Bookmark98 .anchor}**3 - Store/secure shared secret**

**This section is intended for the Customer's implementation team.**

We strongly advise customers to save the shared secret or the signing
certificate in a secure and protected store (such as Vault)

[]{#Bookmark99 .anchor}**4 - Test**

When you are ready to test, Bottomline will trigger a CRC (Challenge
Response Check) process to verify that you are the owner of the webhook
URL.

**Troubleshooting**

-   [Debugging](#debugging)

-   [Logging](#logging)

-   [Failed notifications](#failed-notifications)

    -   [How to identify failed
        notifications?](#how-to-identify-failed-notifications)

    -   [How to resend a notification?](#how-to-resend-a-notification)

    -   [How to check if the notification was successfully
        resent?](#how-to-check-if-the-notification-was-successfully-resent)

-   [Support](#support)

    -   [Integration Support](#integration-support)

    -   [Production Support](#production-support)

-   [Request and Response Examples](#request-and-response-examples)

# **Debugging**

Webhooks can change the logging level for specific packages without
re-building the app. It is possible by changing values.yaml in gitops,
committing it and sync argo-cd. Below is the sample of how it looks like
in staging where we turned to debug level for BTIQ classes:

**values.yaml**

logging:

level:

com.bottomline: DEBUG

DEBUG or lower logging level may expose sensitive data to log so it must
not be enabled in prod.

# **Logging**

**DOCUMENT HERE HOW TO ACCESS LOGS, including ELK URL**

# **Failed notifications**

When the automatic retries process has failed after four attempts to
send the notification to the customer end-point, notifications are stuck
in the Webhooks service with a failed status. You should monitor the
failed notifications and probably do a manual retry.

### How to identify failed notifications?

The *GET 1/webhooks/subscriptions/{subscriptionId}/notifications* API
returns the list of notifications related to a subscription

Attributes of notification are:

-   notification ID

-   HTTP code (as returned by the customer)

-   created date

-   updated date

-   number of retries 

-   event type

-   tenant ID

All notifications having an HTTP status other than 2xx can be considered
"failed notifications".

### How to resend a notification?

The *POST /1/webhooks/notifications/{notificationId}* API allows to
resend a notification

This API only returns a 204, you will need to check notifications again
to get their status

We advise using
the [outbox ](https://confluence.bottomline.tech/display/SPFM/Webhooks+-+Misc#Outbox)library
to implement async calls and guarantee delivery.

### How to check if the notification was successfully resent?

After once we identify the failed notification and when we do the [*GET
1/webhooks/notifications/{notificationId}*](https://confluence.bottomline.tech/download/attachments/348848471/check.svg?version=2&modificationDate=1662024208000&api=v2)
API returns the notification info with 200 HTTP code response :

-   notification ID

-   created date

-   updated date

-   httpCode

-   numRetries

-   eventType

-   tenantId

-   productId

-   subscriptionId

-   status

-   idempotencyKey

-   ***failCount ***

-   state

# **Support**

## **Integration Support**

-   Troubleshooting

    -   Visibility into logs

-   Onboarding

    -   Support calls

    -   Documentation

    -   Testing Support

    -   Demo Support

    -   Sandbox environment

-   Networking Tasks

    -   Whitelist client webhook URL - (Security tasks are required)

    -   Identify & Provide all necessary client secrets and deliverables

-   Planning

    -   Testing Timelines

    -   Environment Readiness

    -   Resource Planning

## **Production Support**

-   Troubleshooting

    -   Visibility Into logs

    -   Testing Resources

    -   Development resources

-   Kibana Logs can be searched as mentioned in the
    [page](https://confluence.bottomline.tech/display/BTIQ/Accessing+Logs+in+Kibana+for+Troubleshooting)

-   Networking

-   Customer Mgt

# **Request and Response Examples**

**Retry sending notification**

**Parameters (set in the URL)**

  Id   ID of notification   UUID type.
  ---- -------------------- ------------

**Returns**

Empty body

**Endpoint**

  POST /webhooks/1/webhooks/notifications/{notification_id}
  -----------------------------------------------------------

**Response**

Empty body

**Miscellaneous**

-   [**Nonfunctional Requirements**](#nonfunctional-requirements)

-   [**Technical Architecture &
    Design**](#technical-architecture-design)

-   [**Certification**](#certification)

-   [**Future Enhancements- North
    Star**](#future-enhancements--north-star)

-   [**API Specifications**](#api-specifications)

-   [**Webhooks SDK**](#webhooks-sdk)

-   [**Idempotency**](#idempotency)

-   [**Outbox**](#outbox)

# Nonfunctional Requirements

[Webhooks
Service](https://confluence.bottomline.tech/display/HSTNG/Webhooks+Service) -
Hosting SLAs and severity mapping

# **Technical Architecture & Design**

[Webhooks
Core](https://confluence.bottomline.tech/display/BTIQ/Webhooks+Core) -
Technical documentation, environment IPs, security requirements, and
more

# **Certification**

[Webhooks - BTIQ Platform
Certification](https://confluence.bottomline.tech/display/BTIQ/Webhooks+-+BTIQ+Platform+Certification)

The webhooks service is c*ertified as Operations Ready* for being a part
of the Shared Platform Core Services.

# **Future Enhancements- North Star**

We want webhooks to be a self-service capability. Customers shall be
able to onboard themselves without any intervention from the Bottomline
implementation team.

# **API Specifications**

[[Download API
specifications]{.underline}](https://btiq-stable.saas-n.com/webhooks/v3/api-docs)

# **Webhooks SDK**

The webhooks SDK is a Java library consumable by spring projects. It
provides an interface to the\
webhooks API. To test the SDK, we have created a sample spring project
([*https://gitlab.saas-p.com/btiq/core-platform-services/client/webhooks-sdk*](https://gitlab.saas-p.com/btiq/core-platform-services/client/webhooks-sdk))\
which imports the SDK and exposes each action of the SDK as a RESTful
method.

Webhooks endpoints can be hit/used with webhooks SDK instead of calling
the APIs directly. The following APIs are available via the SDK

1.  Posting a subscription
    [*[POST]{.underline}* 1/webhooks/createSubscription](https://confluence.bottomline.tech/pages/createpage.action?spaceKey=BTIQ&title=POST+1%2Fwebhooks%2FcreateSubscription)

2.  Posting an
    event *[POST 1/webhooks/postEvent](https://confluence.bottomline.tech/pages/createpage.action?spaceKey=BTIQ&title=POST+1%2Fwebhooks%2FpostEvent) *

3.  Posting an update subscription
    [*POST 1/webhooks/updateSubscription/{subscriptionId}  *](https://confluence.bottomline.tech/pages/createpage.action?spaceKey=BTIQ&title=POST+1%2Fwebhooks%2FupdateSubscription%2F%7BsubscriptionId)

4.  Posting a list of notifications for a subscription
    [*POST 1/webhooks/getNotificationsForSubscription/{subscriptionId} *](https://confluence.bottomline.tech/pages/createpage.action?spaceKey=BTIQ&title=POST+1%2Fwebhooks%2FgetNotificationsForSubscription%2F%7BsubscriptionId)

5.  Posting a get specific notification
    [*POST 1/webhooks/getNotificationById/{subscriptionId}*](https://confluence.bottomline.tech/pages/createpage.action?spaceKey=BTIQ&title=POST+1%2Fwebhooks%2FgetNotificationById)

6.  Posting a get specific subscription [*POST
    1/webhooks/getSubscription/{subscriptionId}*](https://confluence.bottomline.tech/pages/createpage.action?spaceKey=BTIQ&title=POST+1%2Fwebhooks%2FgetSubscription%2F%7BsubscriptionId)

7.  Posting a delete subscription [*[POST
    1/webhooks/unsubscribe/{subscriptionId}]{.underline}*](https://confluence.bottomline.tech/pages/createpage.action?spaceKey=BTIQ&title=POST+1%2Fwebhooks%2Funsubscribe%2F%7BsubscriptionId)

8.  Posting a CRC [*[POST
    1/webhooks/sendChallengeResponseCheck/{subscriptionId}]{.underline}*](https://confluence.bottomline.tech/pages/createpage.action?spaceKey=BTIQ&title=POST+1%2Fwebhooks%2FsendChallengeResponseCheck%2F%7BsubscriptionId)

9.  Posting a list of CRC history
    [*[POST1/webhooks/getChallengeResponseCheckHistory/{subscriptionId}]{.underline}*](https://confluence.bottomline.tech/pages/createpage.action?spaceKey=BTIQ&title=POST1%2Fwebhooks%2FgetChallengeResponseCheckHistory%2F%7BsubscriptionId)

10. Posting a get CRC status [*[POST
    1/webhooks/getChallengeResponseCheckStatus/{subscriptionId}]{.underline}*](https://confluence.bottomline.tech/pages/createpage.action?spaceKey=BTIQ&title=POST+1%2Fwebhooks%2FgetChallengeResponseCheckStatus%2F%7BsubscriptionId)

# **Idempotency**

Webhooks expects an idempotency key in the request for the following
methods:

-   POST  /webhooks/events

-   POST /webhooks/subscriptions

It is presented in the request header: "X-Idempotency-Key" and UUID as
value.

As implementation Webhooks uses the solution provided by the Idempotency
project:
<https://gitlab.saas-p.com/btiq/core-platform-services/common/idempotency>,
see [readme](https://gitlab.saas-p.com/btiq/core-platform-services/common/idempotency/-/blob/master/README.md)
for more details about usage.

Webhooks define specific methods which marked as idempotent with
required data - idempotent key, idempotent request, tenant id, for
instance:

[\@Idempotent]{.citation cites="Idempotent"}

public void eventOccurs([\@IdempotentKey]{.citation
cites="IdempotentKey"} String idempotencyKey,
[\@IdempotentRequest]{.citation cites="IdempotentRequest"} final
EventModel eventModel, [\@TenantID]{.citation cites="TenantID"} String
tenant) {

log.info("EVENT TRIGGERED");

Event event = eventMapper.modelToEntity(eventModel,
UUID.fromString(tenant), scopeHelper.getScopeId());

eventRepository.save(event);

In posting a request to a subscriber Webhooks creates its idempotency
key and sends it to the subscriber:

public int postToSubscriber(EventModel eventModel, String url, String
sharedSecret, String idempotencyKey, EventLogMetadata eventLogMetadata,
boolean isNonRepudiation) {

try {

String payload = eventModel.getPayload();

final var request = Request.Post(url)

.bodyString(payload, ContentType.APPLICATION_JSON)

.addHeader(IDEMPOTENCY_KEY_NAME, idempotencyKey);

# **Outbox**

Webhooks use async calls for two methods:

-   Triggering event - /webhooks/events

-   Triggering CRC event - webhooks/subscriptions/{subscription_id}/crc

As a solution for async calls, we use Outbox lib, which implements async
calls and guarantees the delivery of data to consumers (in our case
posting data to the subscriber URL). For more details:  [Click
Here](https://gitlab.saas-p.com/btiq/core-platform-services/common/btiq-outbox/-/blob/master/README.md)

Based on that, there are two correspondent processors, which are called
in the async way:

-   WebhooksEventProcessor

-   CrcEventProcessor

The outbox table is extended by webhooks specific data:

[\@Builder]{.citation cites="Builder"}

[\@NoArgsConstructor]{.citation cites="NoArgsConstructor"}

[\@AllArgsConstructor]{.citation cites="AllArgsConstructor"}

[\@Entity]{.citation cites="Entity"}

[\@Table]{.citation cites="Table"}(name = "OUTBOX_EVENT")

[\@EntityListeners]{.citation
cites="EntityListeners"}(AuditingEntityListener.class)

public class OutboxEvent extends AbstractEvent {

[\@Column]{.citation cites="Column"}(name = "HTTP_CODE")

private int httpCode;

[\@Column]{.citation cites="Column"}(name = "NUM_RETRIES")

private int numRetries;

[\@OneToOne]{.citation cites="OneToOne"}(targetEntity =
Subscription.class)

[\@JoinColumn]{.citation cites="JoinColumn"}(name = "SUBSCRIPTION_ID",
referencedColumnName = "ID")

private Subscription subscription;

[\@Column]{.citation cites="Column"}(name = "STATUS")

private String status;

In the next version, we refactored that and moved all webhooks related
data from the outbox table, it allows us to use the latest version of
outbox.

**FAQ**

**Our tenantIds are just strings, not UUIDs. What shall we do?**\
You need to add a UUID to your tenant entity.

**How does BTIQ manage failures?**\
BTIQ manages automatic retries. But if the last retry fails, LOBs can
implement exception management. For more details: [Click
here](https://confluence.bottomline.tech/display/BTIQ/Manual+retry+of+webhooks+notifications)

**How can we test?**

*\<TBD\>*

**Do we have a swagger for customers to receive events?**\
The product line defines the format. BTIQ sends the payload to the
customer without any transformation. We advise adding the event type in
the payload if the customers use the same URL for multiple event types.

**How can we create a new event type?**\
Event types are free text for the moment; you can set the value you
want. BTIQ advise prefixing event types by the solution to avoid
duplicates among product lines. BTIQ provides a repository to define
event types linked to features.

**What is the productid?**\
The productid is a UUID for your solution. The productid should be the
parentID generated along with the Scope ID. There is only one parentID.

**How can customers ensure security and integrity?**\
CRCs provide a security layer in case of a security incident at the
customer application or DNS infrastructure by limiting sensitive data
exposure. BTIQ computes a signature based on the payload using the
shared secret (passed in the Signature header field) on each POST
request. Customers can check the signature to confirm that Bottomline is
the source of the incoming webhooks and ensure that the Webhooks payload
has not been modified in transit (integrity check). Customers are
strongly encouraged to implement IP address filtering to minimize the
attack surface.

**How to manage a CRC process?**\
When a Product line triggers a CRC process for a given subscription, the
webhooks service requests the customer's webhook URL with a *crc_token*
parameter. The customer application replies with an encrypted
*response_token* based on the *crc_token* parameter and the shared
secret. Then the LOBs can check the CRCs' status and take appropriate
actions if the customer's reply is not as expected.
