WooCommerce API Unofficial - Python Client
===============================

**Having no feedback from the creator of wc-api-python, I have forked the project to add the session functionality of requests and the Retry functionality**

A Python wrapper for the WooCommerce REST API. Easily interact with the WooCommerce REST API using this library.

.. image:: https://github.com/woocommerce/wc-api-python/actions/workflows/ci.yml/badge.svg?branch=trunk
    :target: https://github.com/woocommerce/wc-api-python/actions/workflows/ci.yml

.. image:: https://img.shields.io/pypi/v/woocommerce.svg
    :target: https://pypi.python.org/pypi/WooCommerce


Installation
------------

Use the code directly in your project. There is no pip package for this unofficial Woocommerce API.

Getting started
---------------

Generate API credentials (Consumer Key & Consumer Secret) following this instructions http://woocommerce.github.io/woocommerce-rest-api-docs/#rest-api-keys.

Check out the WooCommerce API endpoints and data that can be manipulated in http://woocommerce.github.io/woocommerce-rest-api-docs/.

Setup
-----

.. code-block:: python

    from woocommerce import API

    wcapi = API(
        url="http://example.com",
        consumer_key="ck_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        consumer_secret="cs_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        version="wc/v3"
    )

Options
~~~~~~~

+-----------------------+-------------+----------+-------------------------------------------------------------------------------------------------------+
|         Option        |     Type    | Required |                                              Description                                              |
+=======================+=============+==========+=======================================================================================================+
| ``url``               | ``string``  | yes      | Your Store URL, example: http://woo.dev/                                                              |
+-----------------------+-------------+----------+-------------------------------------------------------------------------------------------------------+
| ``consumer_key``      | ``string``  | yes      | Your API consumer key                                                                                 |
+-----------------------+-------------+----------+-------------------------------------------------------------------------------------------------------+
| ``consumer_secret``   | ``string``  | yes      | Your API consumer secret                                                                              |
+-----------------------+-------------+----------+-------------------------------------------------------------------------------------------------------+
| ``version``           | ``string``  | no       | API version, default is ``wc/v3``                                                                     |
+-----------------------+-------------+----------+-------------------------------------------------------------------------------------------------------+
| ``timeout``           | ``integer`` | no       | Connection timeout, default is ``5``                                                                  |
+-----------------------+-------------+----------+-------------------------------------------------------------------------------------------------------+
| ``verify_ssl``        | ``bool``    | no       | Verify SSL when connect, use this option as ``False`` when need to test with self-signed certificates |
+-----------------------+-------------+----------+-------------------------------------------------------------------------------------------------------+
| ``query_string_auth`` | ``bool``    | no       | Force Basic Authentication as query string when ``True`` and using under HTTPS, default is ``False``  |
+-----------------------+-------------+----------+-------------------------------------------------------------------------------------------------------+
| ``user_agent``        | ``string``  | no       | Set a custom User-Agent, default is ``WooCommerce-Python-REST-API/3.0.0``                             |
+-----------------------+-------------+----------+-------------------------------------------------------------------------------------------------------+
| ``oauth_timestamp``   | ``integer`` | no       | Custom timestamp for requests made with oAuth1.0a                                                     |
+-----------------------+-------------+----------+-------------------------------------------------------------------------------------------------------+
| ``retries``           | ``int``     | no       | Set to ``3`` in order to retry 3 times before exiting.                                                |
+-----------------------+-------------+----------+-------------------------------------------------------------------------------------------------------+
| ``backoff_factor``    | ``float``   | no       | Set to ``0.3``. Change how long the processes will sleep between failed requests (exponential).       |
+-----------------------+-------------+----------+-------------------------------------------------------------------------------------------------------+
| ``status_forcelist``  | ``list``    | no       | Set to ``[500, 502, 503, 504, 429]`` List of status on which we have to try again                     |
+-----------------------+-------------+----------+-------------------------------------------------------------------------------------------------------+

Methods
-------

+--------------+----------------+------------------------------------------------------------------+
|    Params    |      Type      |                           Description                            |
+==============+================+==================================================================+
| ``endpoint`` | ``string``     | WooCommerce API endpoint, example: ``customers`` or ``order/12`` |
+--------------+----------------+------------------------------------------------------------------+
| ``data``     | ``dictionary`` | Data that will be converted to JSON                              |
+--------------+----------------+------------------------------------------------------------------+
| ``**kwargs`` | ``dictionary`` | Accepts ``params``, also other Requests arguments                |
+--------------+----------------+------------------------------------------------------------------+

GET
~~~

- ``.get(endpoint, **kwargs)``

POST
~~~~

- ``.post(endpoint, data, **kwargs)``

PUT
~~~

- ``.put(endpoint, data), **kwargs``

DELETE
~~~~~~

- ``.delete(endpoint, **kwargs)``

OPTIONS
~~~~~~~

- ``.options(endpoint, **kwargs)``

Response
--------

All methods will return `Response <http://docs.python-requests.org/en/latest/api/#requests.Response>`_ object.

Example of returned data:

.. code-block:: bash

    >>> r = wcapi.get("products")
    >>> r.status_code
    200
    >>> r.headers['content-type']
    'application/json; charset=UTF-8'
    >>> r.encoding
    'UTF-8'
    >>> r.text
    u'{"products":[{"title":"Flying Ninja","id":70,...' // Json text
    >>> r.json()
    {u'products': [{u'sold_individually': False,... // Dictionary data

Request with `params` example
-----------------------------

.. code-block:: python

    from woocommerce import API

    wcapi = API(
        url="http://example.com",
        consumer_key="ck_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        consumer_secret="cs_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        version="wc/v3"
    )

    # Force delete example.
    print(wcapi.delete("products/100", params={"force": True}).json())

    # Query example.
    print(wcapi.get("products", params={"per_page": 20}).json())


Changelog
---------

See `CHANGELOG.md <https://github.com/woocommerce/wc-api-python/blob/trunk/CHANGELOG.md>`_.
