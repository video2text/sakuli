How to Fetch and Filter Real-Time Train Station Data Using SNCF’s API
======================================================================

Learn how to query specific train stations, refine by city or region, and filter using ODSQL to get clean, structured data for your application or research.

Overview
--------

SNCF provides a powerful Open Data API that allows developers and researchers to access up-to-date data on French train stations. In this guide, we’ll cover how to:

- Query station data from a specific dataset
- Filter stations by city or region using ODSQL
- Retrieve only the fields you need
- Interpret and use the structured JSON response

Base URL
--------

The base URL for the SNCF Open Data API is:

::

    https://ressources.data.sncf.com/api/explore/v2.1/catalog

Step 1: Identify the Dataset
-----------------------------

To fetch train station data, you first need to know the dataset ID. For example, the dataset for **Gares et points d’arrêt** (stations and stops) is commonly:

::

    ressources.data.sncf.com/explore/dataset/gares

API path for accessing records:

::

    GET /datasets/gares/records

Step 2: Refine by City or Region
--------------------------------

You can use the ``refine`` query parameter to limit results by city.  
Example: To get all stations in **Paris**:

::

    GET /datasets/gares/records?refine=city:Paris

Refine by both region and station type:

::

    GET /datasets/gares/records?refine=region:Île-de-France&refine=type:Train

Step 3: Filter with ODSQL
-------------------------

For more advanced filtering, use the ``where`` parameter and Opendatasoft’s Query Language (ODSQL).  
Example: Fetch stations in cities with population over 50,000:

::

    GET /datasets/gares/records?where=population>50000

Step 4: Select Specific Fields
------------------------------

Use the ``select`` parameter to return only the fields you need.  
This improves performance and response readability.

Example:

::

    GET /datasets/gares/records?select=name,coordinates,city

Step 5: Full Example Request
----------------------------

Combine all features in a single request:

::

    GET /datasets/gares/records?select=name,coordinates,city&refine=region:Occitanie&where=type="Train"&limit=10

Example JSON response:

.. code-block:: json

    {
      "total_count": 120,
      "results": [
        {
          "name": "Toulouse Matabiau",
          "coordinates": {
            "lat": 43.611,
            "lon": 1.454
          },
          "city": "Toulouse"
        }
      ]
    }

Tips and Best Practices
-----------------------

- Use the official `API Console <https://ressources.data.sncf.com/api/explore/v2.1/console>`_ to test queries.
- Use `offset` and `limit` for pagination on large datasets.
- Combine ``refine`` and ``where`` for more precise filters.
- Use ``lang=en`` to retrieve English field names (if available).

Conclusion
----------

The SNCF Open Data API is a robust and flexible tool for accessing real-time station information.  
By leveraging query parameters and filters, you can tailor the response to your exact needs, whether for mapping, data analysis, or transit applications.

References
----------

- API Console: https://ressources.data.sncf.com/api/explore/v2.1/console
- ODSQL Language Reference: https://docs.opendatasoft.com/en/data_exploration/04_analyzing_data/03_using_query_language.html
- SNCF Dataset Portal: https://ressources.data.sncf.com/pages/home/
