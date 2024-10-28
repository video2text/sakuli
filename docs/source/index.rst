RTA Dubai Bus Journey Planner API
^^^^^^^^
Overview

The RTA Dubai Bus Journey Planner API provides access to real-time bus schedules and timetables for Dubai's public transportation system. This documentation outlines the available endpoints, request parameters, and response formats for accessing bus timing information.

Retrieves the timetable for a specific bus route.

Endpoint
^^^^^^^^
::

    GET /DownloadTimetableServlet

Query Parameters
^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: 15 10 10 45 20

   * - Parameter
     - Type
     - Required
     - Description
     - Example
   * - lineId
     - string
     - Yes
     - Unique identifier for the bus route
     - dub:01SH1:%20:H:y08
   * - lineName
     - string
     - Yes
     - Display name of the bus route
     - bus%20SH1

Route ID Format
^^^^^^^^^^^^^^
The ``lineId`` parameter follows this format: ``dub:{route_code}:%20:H:y08``

* ``dub``: City identifier for Dubai
* ``route_code``: Specific code for the bus route
* ``:%20:H:y08``: System-specific suffix

Available Routes
^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: 10 30 20 40

   * - Route
     - Line ID
     - Line Name
     - Description
   * - SH1
     - dub:01SH1:%20:H:y08
     - bus%20SH1
     - SH1 Route Service
   * - D03
     - dub:01D03:%20:H:y08
     - bus%20D03
     - D03 Route Service
   * - E411
     - dub:10411:%20:H:y08
     - bus%20E411
     - E411 Route Service
   * - F62
     - dub:12F62:%20:H:y08
     - bus%20F62
     - F62 Route Service

Example Request
^^^^^^^^^^^^^
.. code-block:: bash

    curl -X GET "https://www.rta.ae/wps/PA_JourneyPlanner/DownloadTimetableServlet?lineId=dub:01SH1:%20:H:y08&lineName=bus%20SH1"

Response
^^^^^^^^
The API returns a timetable document containing the bus schedule information.

Response Headers
^^^^^^^^^^^^^^^
::

    Content-Type: application/pdf

Error Codes
^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: 20 80

   * - Status Code
     - Description
   * - 200
     - Success - Timetable retrieved successfully
   * - 400
     - Bad Request - Invalid parameters
   * - 404
     - Not Found - Route not found
   * - 500
     - Internal Server Error - Server-side error occurred

Best Practices
-------------
1. Cache the timetable responses when possible to reduce server load
2. Implement error handling for failed requests
3. Include proper timeout handling in your implementation
4. Use URL encoding for the lineName parameter

Rate Limiting
------------
* Default rate limit: Not specified
* It's recommended to implement reasonable request intervals to avoid overloading the server

Notes
-----
1. The timetable data is provided in PDF format
2. Timetables may be updated periodically by RTA
3. All times are in Gulf Standard Time (GST/UTC+4)
4. Service availability may vary during holidays and special events

Example Implementation
---------------------

JavaScript
~~~~~~~~~~
.. code-block:: javascript

    async function getRTABusTimetable(routeCode, routeName) {
      try {
        const baseUrl = 'https://www.rta.ae/wps/PA_JourneyPlanner/DownloadTimetableServlet';
        const lineId = `dub:${routeCode}:%20:H:y08`;
        const lineName = `bus%20${routeName}`;
        
        const response = await fetch(
          `${baseUrl}?lineId=${lineId}&lineName=${lineName}`,
          {
            method: 'GET',
            headers: {
              'Accept': 'application/pdf'
            }
          }
        );

        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }

        const pdfBlob = await response.blob();
        return pdfBlob;
      } catch (error) {
        console.error('Error fetching timetable:', error);
        throw error;
      }
    }

Python
~~~~~~
.. code-block:: python

    import requests

    def get_rta_bus_timetable(route_code, route_name):
        base_url = 'https://www.rta.ae/wps/PA_JourneyPlanner/DownloadTimetableServlet'
        
        params = {
            'lineId': f'dub:{route_code}:%20:H:y08',
            'lineName': f'bus%20{route_name}'
        }
        
        try:
            response = requests.get(base_url, params=params)
            response.raise_for_status()
            
            return response.content
        except requests.exceptions.RequestException as e:
            print(f"Error fetching timetable: {e}")
            raise

    # Example usage
    try:
        pdf_content = get_rta_bus_timetable('01SH1', 'SH1')
        with open('timetable.pdf', 'wb') as f:
            f.write(pdf_content)
    except Exception as e:
        print(f"Failed to download timetable: {e}")

Support and Feedback
-------------------
For technical support or API-related questions, please contact RTA's technical support team.

Version History
--------------
* Current Version: 1.0
* Last Updated: 2024

Legal Notice
-----------
This API documentation is provided for informational purposes. Usage of the RTA Dubai Bus Journey Planner API is subject to RTA's terms of service and data usage policies.
