Introduction
================

The API enables you to submit an order to Send2China server, which on
success returns a summary of the order including its ID, or on failure
returns error messages. The order ID can be used to query the server
at a later time to get the URLs of the order's documents.

An order can be thought of as a group of parcels from the same sender
using the same service. A valid order must have all required
fields and all fields must pass constraint checks.

A valid order with sufficient funds in the user's account will produce
shippping labels, invoices, etc. on Send2China server. Depending on
the service chosen, Send2China server may need to asynchronously make
API calls to the service provider. In that case the order's documents
are not immediately available and a delay must be implemented on the
user's end after placing an order. Alternatively, you can expect an
email with the documents just like any order made on the website.

The API follows common practices of RESTful HTTP web services. To send
data you POST json to an API endpoint, and to retrieve data you GET
json from an API endpoint.


API Endpoint
----------------

All API URLs are relative to the root endpoint on the two web servers:

- Test server: \http://162.243.85.198/api/|version|
- Production server: \https://send2china.co.uk/api/|version|

Production server API can only be accessed via https.


Supported methods
--------------------

===============      ============
Resource             Description
===============      ============
GET /orders/         Return a list of orders
POST /orders/        Create and pay for an order
GET /orders/id/      Get an order's details
GET /products/       Return a list of products
===============      ============


Authentication
--------------------

Currently only `HTTP Basic Authentication
<http://tools.ietf.org/html/rfc2617>`_ is supported. Users sign in
with their username and password. Other authentication methods are
being worked on.


Testing API
--------------------

The API provides a human-friendly HTML interface. If an endpoint
supports POST, you can open it in a web browser, scroll to the bottom
of the page and submit raw json data. If you are not yet logged into
our website, you must do so by clicking the *Log in* link in the upper
right corner.

As a first step, try to log into
`http://162.243.85.198/api/1.0/orders/
<http://162.243.85.198/api/1.0/orders/>`_. Read on to find a json data
example which can be POSTed either in a web browser or in raw HTTP requests.
