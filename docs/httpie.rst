A short example
================

Creating an order
-----------------

Below is the JSON file we will be posting to the server.

.. code-block:: json
   :caption: order.json

   {
       "product_id": 1,
       "name": "zhang san",
       "street": "2",
       "street2": "3",
       "city": "6",
       "postcode": "BN12 4HF",
       "phone": "9",
       "additional_instructions": "",
       "pickup_time": "2014-05-05T10:00:00",
       "consignments": [
          {
               "name": "LIU DE HUA",
               "street": "XIANG GANG JIU LONG BAN",
               "street2": "SHAN HAO ZHAI 1 DONG FA",
               "street3": "",
               "city": "HONGKONG",
               "postcode": "991114",
               "country": "CN",
               "phone": "99111449077",
               "cn_name": "",
               "cn_city": "",
               "cn_street": "",
               "packets": [
                 {
                     "weight": 1.0,
                     "length": 2,
                     "width": 3,
                     "height": 4,
                     "contents": [
                         {
                             "type": "Baby milk powder",
                             "quantity": 1,
                             "cost": "2.00"
                         }
                     ]
                 }
                ]
          }
       ]
   }

After creating this file, we use HTTPie, a command line, cURL-like
tool to make HTTP requests. Alternatively, you can also interact with
the server in a web browser.

.. code-block:: sh

   $ http -b -a api-test:apitest POST \
       http://162.243.85.198/api/1.0/orders/ < order.json

Here we login as ``api-test`` whose password is ``apitest``. This
account has a few hunderd pounds of deposit for testing purposes. The
server returns json data similar to below, except the ``id`` will be a
different, unique number.

.. code-block:: json

    {
        "additional_instructions": "",
        "city": "6",
        "consignments": [
            {
                "packets": [
                    {
                        "city": "HONGKONG",
                        "cn_city": "",
                        "cn_name": "",
                        "cn_street": "",
                        "contents": [
                            {
                                "cost": "2.00",
                                "quantity": 1,
                                "type": "Baby milk powder"
                            }
                        ],
                        "height": 4,
                        "length": 2,
                        "name": "LIU DE HUA",
                        "phone": "99111449077",
                        "postcode": "991114",
                        "street": "XIANG GANG JIU LONG BAN",
                        "street2": "SHAN HAO ZHAI 1 DONG FA",
                        "street3": "",
                        "weight": 1.0,
                        "width": 3
                    }
                ]
            }
        ],
        "id": 9390,
        "label_url": "/static/labels/9390-57ab72f4-e852-40d3-85ec-499086d254b5-label.pdf",
        "name": "zhang san",
        "phone": "9",
        "pickup_time": "2014-05-05T10:00:00",
        "postcode": "BN12 4HF",
        "product_id": 1,
        "street": "2",
        "street2": "3",
        "street3": "",
        "total": "13.63"
    }

In the above the ``http`` command we passed ``-b`` to show the
response body only. Your program, however, must check the HTTP status code in
the response header which indicates success or failure of the order. If
the order failed, the response body will contain helpful error messages.

On success the server returns the order ``id`` and the
``label_url``. The ``label_url`` lets you download the label from
Send2China server. Note that the label generation will take some time
while the order is being queued to be processed. Usually the label
becomes available within a few minutes, but if for any reason, the
label does not appear after a reasonable amount of delay, please
contact Send2China administrators.

Retrieving an order
-------------------

Once we have the order ``id``, we can query the server for its status,
e.g. the label availability, and the tracking numbers.

.. code-block:: sh

   $ http -b GET http://162.243.85.198/api/1.0/orders/9390/ \
      "Authorization: Token 2886a473a9eff33e6b0268e3528fcc88857492aa"

Note that the trailing ``/`` is required, or a 301 Redirect will be
returned. Instead of passing my username and password, here we obtained
API token and showed its usage.

.. code-block:: json

   {
    "additional_instructions": "",
    "city": "6",
    "consignments": [
        {
            "packets": [
                {
                    "city": "HONGKONG",
                    "cn_city": "",
                    "cn_name": "",
                    "cn_street": "",
                    "contents": [
                        {
                            "cost": "2.00",
                            "quantity": 1,
                            "type": "Baby milk powder"
                        }
                    ],
                    "country": "CN",
                    "height": 4,
                    "length": 2,
                    "name": "LIU DE HUA",
                    "phone": "99111449077",
                    "postcode": "991114",
                    "street": "XIANG GANG JIU LONG BAN",
                    "street2": "SHAN HAO ZHAI 1 DONG FA",
                    "street3": "",
                    "tracking_inner": "",
                    "tracking_outer": "EA913905781BE",
                    "weight": 1.0,
                    "width": 3
                }
            ]
        }
    ],
    "id": 9390,
    "label_url": "/static/labels/9390-57ab72f4-e852-40d3-85ec-499086d254b5-label.pdf",
    "name": "zhang san",
    "phone": "9",
    "pickup_time": "2014-05-05T10:00:00",
    "postcode": "BN12 4HF",
    "processed": true,
    "product_id": 1,
    "street": "2",
    "street2": "3",
    "street3": "",
    "total": "13.63"
  }

The returned data now has an extra field ``processed`` for the order,
to indicate if the label is available. For each packet there are also
two extra fields ``tracking_outer`` and ``tracking_inner``. The former
is the international tracking number, and the latter is the domestic
tracking number. The domestic one is useful when the product has a
separate domestic service in addition to the international service.

Next, we will define order formally.
