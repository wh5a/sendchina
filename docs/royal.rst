Royal Mail
==================

Quickstart
-------------

Create an order by posting the JSON file below to the API server.

.. code-block:: json
   :caption: rmorder.json

   {
       "service": "TPL",
       "name": "Tom",
       "addressLine1": "44-46 Morningside Road",
       "addressLine2": "line2",
       "reference": "ref",
       "town": "Edinburgh",
       "postcode": "EH10 4BF",
       "weight": 100
   }

.. code-block:: sh

   $ http -b -a api-test:apitest http://162.243.85.198/api/1.0/royal/ < rmorder.json

Retrieve an order using the order id returned from the server.

.. code-block:: sh

   $ http -b -a api-test:apitest http://162.243.85.198/api/1.0/royal/10/


.. code-block:: json

   {
       "addressLine1": "44-46 Morningside Road",
       "addressLine2": "line2",
       "addressLine3": "",
       "business": "",
       "error": "",
       "id": 10,
       "label_url": "/static/labels/10-bc639842-f128-4392-a5d9-13d834acbcab-TTT006209070GB-label.pdf",
       "name": "Tom",
       "postcode": "EH10 4BF",
       "reference": "ref",
       "service": "TPL",
       "town": "Edinburgh",
       "tracking": "TTT006209070GB",
       "weight": 100
   }

Formal Definition
-----------------

Parameters marked with * are required when creating a Royal Mail
order. Read-only parameters are ignored in POST and only useful in
GET.

+-----------------------+-------+--------------------------------------------------------------------+
|Parameter              |Type   |Description                                                         |
+=======================+=======+=======================+============================================+
|id                     |integer|Unique id of an order. |Read only                                   |
+-----------------------+-------+-----------------------+                                            |
|label_url              |string |Path of the shipping   |                                            |
|                       |       |label on the web server|                                            |
+-----------------------+-------+-----------------------+                                            |
|error                  |string |Error returned from the|                                            |
|                       |       |upstream server        |                                            |
+-----------------------+-------+-----------------------+                                            |
|tracking               |string |Tracking number        |                                            |
+-----------------------+-------+-----------------------+                                            |
|total                  |float  |Total cost in pounds   |                                            |
+-----------------------+-------+-----------------------+--------------------------------------------+
|**service***           |string |TPL or TRL                                                          |
+-----------------------+-------+--------------------------------------------------------------------+
|**name***              |string |Recipient name                                                      |
|                       |max    |                                                                    |
|                       |length:|                                                                    |
|                       |80     |                                                                    |
+-----------------------+-------+--------------------------------------------------------------------+
|business               |string |Recipient business name                                             |
|                       |max    |                                                                    |
|                       |length:|                                                                    |
|                       |64     |                                                                    |
+-----------------------+-------+--------------------------------------------------------------------+
|reference              |string |The user can supply their own reference number                      |
|                       |max    |                                                                    |
|                       |length:|                                                                    |
|                       |20     |                                                                    |
+-----------------------+-------+--------------------------------------------------------------------+
|**addressLine1***      |max    |Recipient address line 1                                            |
+-----------------------+length:+--------------------------------------------------------------------+
|addressLine2           |35     |Recipient address line 2/3                                          |
+-----------------------+       |                                                                    |
|addressLine3           |       |                                                                    |
+-----------------------+       +--------------------------------------------------------------------+
|**town***              |       |Recipient town                                                      |
+-----------------------+-------+--------------------------------------------------------------------+
|**postcode***          |string |Recipient postcode (UK only)                                        |
+-----------------------+-------+--------------------------------------------------------------------+
|**weight***            |integer|Weight (grams)                                                      |
+-----------------------+-------+--------------------------------------------------------------------+
