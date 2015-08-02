Formal definition
==================

Parameters marked with * are required when creating an
order. Read-only parameters are ignored in POST and only useful in
GET. Unless otherwise specified, strings are ASCII only.

Order
---------------

+-----------------------+-------+--------------------------------------------------------------------+
|Parameter              |Type   |Description                                                         |
+=======================+=======+=======================+============================================+
|id                     |integer|Unique id of an order. |Read only                                   |
+-----------------------+-------+-----------------------+                                            |
|label_url              |string |Path of the shipping   |                                            |
|                       |       |label on the web server|                                            |
|                       |       |if an order has been   |                                            |
|                       |       |successfully processed.|                                            |
+-----------------------+-------+-----------------------+                                            |
|total                  |float  |Total cost of an order |                                            |
|                       |       |in pounds.             |                                            |
+-----------------------+-------+-----------------------+--------------------------------------------+
|**product_id***        |integer|Must match a valid product id listed in the                         |
|                       |       |:ref:`/products/<intro>` API endpoint.                              |
|                       |       |                                                                    |
|                       |       |A **product** is a combination of one or more services that ships   |
|                       |       |packets from A to B. A product has an ``id`` and a ``title``, which |
|                       |       |identifies the services included in this product.                   |
|                       |       |                                                                    |
|                       |       |The product list on the production server diverges from the test    |
|                       |       |server. We recommend that you maintain two separate local           |
|                       |       |copies. Because a product ``id`` never changes, the client code only|
|                       |       |needs to be changed when we notify you of addition or removal of    |
|                       |       |products.                                                           |
+-----------------------+-------+--------------------------------------------------------------------+
|**name***              |string |Sender name                                                         |
+-----------------------+       +--------------------------------------------------------------------+
|**street***            |max    |Sender street address line 1                                        |
+-----------------------+length:+--------------------------------------------------------------------+
|street2                |24     |Sender street address line 2/3                                      |
+-----------------------+       |                                                                    |
|street3                |       |                                                                    |
+-----------------------+       +--------------------------------------------------------------------+
|**city***              |       |Sender city                                                         |
+-----------------------+-------+--------------------------------------------------------------------+
|**postcode***          |string |Sender postcode (UK only)                                           |
+-----------------------+-------+--------------------------------------------------------------------+
|**phone***             |string |Sender phone number                                                 |
|                       |       |                                                                    |
|                       |max    |Only numbers or spaces are allowed.                                 |
|                       |length:|                                                                    |
|                       |15     |                                                                    |
+-----------------------+-------+--------------------------------------------------------------------+
|additional_instructions|string |Instructions for the pickup driver. Usually only useful for         |
|                       |       |collection products.                                                |
|                       |max    |                                                                    |
|                       |length:|                                                                    |
|                       |50     |                                                                    |
+-----------------------+-------+--------------------------------------------------------------------+
|pickup_time            |string |Date and time for collection, ignored for self delievery.           |
|                       |       |                                                                    |
|                       |       |Format: YYYY-MM-DDT00:00:00 Time is usually ignored and drivers may |
|                       |       |come any time on the requested date.                                |
|                       |       |                                                                    |
|                       |       |Collection date must be one of the next three work days not         |
|                       |       |including today.                                                    |
+-----------------------+-------+--------------------------------------------------------------------+
|**consignments***      |array  |List of consignments                                                |
+-----------------------+-------+--------------------------------------------------------------------+


Consignment
-------------

A consignment is a set of packets sent together to the same recipient in China.

+---------------+--------------+-------------------------------------------+
|Parameter      |Type          |Description                                |
+===============+==============+===========================================+
|**name***      |string        |Recipient name                             |
+---------------+              +-------------------------------------------+
|**street***    |max length:   |Recipient street address line 1            |
+---------------+24            +-------------------------------------------+
|street2        |              |Recipient street address line 2/3          |
+---------------+              |                                           |
|street3        |              |                                           |
+---------------+              +-------------------------------------------+
|**city***      |              |Recipient city                             |
+---------------+--------------+-------------------------------------------+
|**postcode***  |6-digit       |Recipient postcode                         |
|               |integer       |                                           |
+---------------+--------------+-------------------------------------------+
|**phone***     |11-digit      |Recipient mobile phone number              |
|               |integer       |                                           |
+---------------+--------------+-----------------------+-------------------+
|cn_name        |string        |Recipient name in      |Chinese address.   |
|               |              |Chinese                |                   |
+---------------+max length: 24+-----------------------+**Required** for   |
|cn_city        |              |Recipient city in      |EMS products.      |
|               |              |Chinese                |                   |
|               |              |                       |Optional for other |
+---------------+--------------+-----------------------+products, allowing |
|cn_street      |string        |Recipient street in    |more accurate      |
|               |              |Chinese                |delievery in China.|
|               |max length: 72|                       |                   |
+---------------+--------------+-----------------------+-------------------+
|cid            |string        |Recipient Chinese Citizen ID number        |
|               |              |                                           |
|               |length: 18    |**Required** by EMS products for new       |
|               |              |recipient. Optional otherwise.             |
+---------------+--------------+-------------------------------------------+
|**packets***   |array         |List of packets.                           |
|               |              |                                           |
|               |              |Parcelforce global priority enforces that a|
|               |              |consignment has no more than three packets.|
|               |              |Other products have no constraint since    |
|               |              |price is charged on individual packets.    |
|               |              |                                           |
+---------------+--------------+-------------------------------------------+


Packet
-------------

+---------------+--------------+-------------------------------------------+
|Parameter      |Type          |Description                                |
+===============+==============+===========================================+
|**weight***    |float         |Weight (kg) < 30kg                         |
+---------------+--------------+-------------------------------------------+
|**length***    |integer       |Dimensions (cm)                            |
+---------------+              |                                           |
|**width***     |              |Every product defines its constraints on   |
+---------------+              |each side and/or perimeter.                |
|**height***    |              |                                           |
+---------------+--------------+-------------------------------------------+
|**contents***  |array         |List of contents                           |
+---------------+--------------+-------------------------------------------+


Content
-----------

Used on customs declaration.

+---------------+--------------+-----------------------+
|Parameter      |Type          |Description            |
+===============+==============+=======================+
|**type***      |string        |Content description    |
|               |              |                       |
|               |max length: 50|                       |
+---------------+--------------+-----------------------+
|**quantity***  |integer       |Quantity               |
+---------------+--------------+-----------------------+
|**cost***      |float         |Unit cost in pounds    |
+---------------+--------------+-----------------------+
