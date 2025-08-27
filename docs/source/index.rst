Sakuli E2E Testing & Monitoring Documentation
=============================================

Version: Open Source  

Base URL: https://sakuli.io

Overview
--------
Sakuli is an open platform for **UI testing, End-to-End (E2E) monitoring, and Robotic Process Automation (RPA)**.  
It allows simulation of real user interactions on both web and native applications, forwarding results to monitoring systems like Prometheus, Icinga, or checkmk.

Authentication
--------------
No authentication is required for using Sakuli locally.  
Enterprise monitoring integration may require configuration (API tokens, system credentials).

Main Features
=============

UI Testing
----------
Automate regression, approval, and functional tests for web and native applications.

**Capabilities:**

- Cross-browser support (Chrome, Firefox).  
- Simplified Selenium DSL (no more StaleElement errors).  
- DOM-based and screenshot-based interaction.  
- Drag & Drop, clipboard usage, auto-scroll, keyboard & mouse simulation.

**Example Script:**

.. code-block:: javascript

   const { By } = require("sakuli/selenium");

   (async () => {
     const driver = await startBrowser();
     await driver.get("https://example.com");
     const element = await driver.findElement(By.css(".login"));
     await element.click();
     console.log("Login button clicked!");
   })();

End-to-End Monitoring
---------------------
Integrate Sakuli tests with monitoring systems to detect performance or availability issues.

**Supported Forwarders:**

- Prometheus  
- Icinga2 / checkmk  
- OMD (Nagios, Gearman)  
- ElasticSearch (coming soon)  
- SQL Databases (coming soon)

**Example Monitoring Output (JSON):**

.. code-block:: json

   {
     "testCase": "Login Workflow",
     "status": "ERROR",
     "executionTime": 12.8,
     "message": "Timeout on login screen",
     "timestamp": "2025-08-27T10:42:00Z"
   }

Robotic Process Automation (RPA)
--------------------------------
Automate workflows and manual tasks across desktop and web apps.

**Example Use Case:**

- Open Windows VM  
- Launch ticket system  
- Copy ticket ID to Excel  
- Validate customer email

Containerized Execution
-----------------------
Sakuli offers a **Docker image** for reproducible and scalable test runs.

**Default Container Includes:**

- Ubuntu OS + OpenBox desktop  
- Node.js runtime  
- Firefox & Chromium browsers  
- VNC & noVNC for live debugging

**Benefits:**

- Same clean environment on each run  
- Parallel test execution  
- Integration with Kubernetes/OpenShift

Response Codes
==============

When integrated with monitoring APIs, Sakuli returns standard statuses:

- **200 OK**: Test executed successfully.  
- **400 Bad Request**: Invalid test configuration.  
- **408 Timeout**: Application response too slow.  
- **500 Internal Error**: Test execution failure.  

**Example Error Response (JSON):**

.. code-block:: json

   {
     "testCase": "Checkout Process",
     "status": "FAILED",
     "message": "Element #submit not found",
     "error_code": "NoSuchElementError"
   }

Runs On
=======

- Windows 10+  
- macOS 10.10+  
- Ubuntu 16.04+  
- OpenShift / Kubernetes  
- AWS, Azure, Google Cloud  

Additional References
=====================

- Official Website: https://sakuli.io  
- Documentation: https://sakuli.io/docs  

.. toctree::
   :maxdepth: 2
   :caption: Fetch and Filter

   practical-use/how-to-fetch-and-filter-real-time-train-station-data
