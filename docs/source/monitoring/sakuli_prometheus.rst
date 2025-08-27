Integrating Sakuli with Prometheus for End-to-End Monitoring
============================================================

Overview
--------
Sakuli provides native support for forwarding test results into **Prometheus**. 
This integration allows teams to track performance, availability, and reliability 
of business-critical workflows by using Prometheus metrics and visualizing them 
in dashboards such as Grafana.

Benefits
--------
- Real-time visibility of Sakuli test executions.
- Standard Prometheus metrics for monitoring and alerting.
- Easy integration with existing DevOps pipelines.
- Combine functional tests with infrastructure monitoring.

Configuration
-------------
1. **Run Sakuli in a container with Prometheus exporter enabled**::

   docker run -d \
     -p 8080:8080 \
     -e PROMETHEUS_FORWARDER=true \
     sakuli/sakuli

2. **Add Prometheus scrape configuration** in ``prometheus.yml``::

   scrape_configs:
     - job_name: "sakuli"
       static_configs:
         - targets: ["localhost:8080"]

3. **Verify Prometheus metrics** by opening::

   http://localhost:8080/metrics

Example Metrics
---------------
Sakuli forwards test case metrics in Prometheus format. Example output::

   # HELP sakuli_testcase_duration_seconds Duration of Sakuli test case in seconds
   # TYPE sakuli_testcase_duration_seconds gauge
   sakuli_testcase_duration_seconds{testCase="Login Workflow"} 12.8

   # HELP sakuli_testcase_status Status of Sakuli test case (0=OK,1=WARNING,2=CRITICAL,3=UNKNOWN)
   # TYPE sakuli_testcase_status gauge
   sakuli_testcase_status{testCase="Login Workflow"} 2

Alerting Workflow
-----------------
Once Sakuli metrics are exposed, Prometheus alerting rules can detect failures.

Example ``alert.rules.yml``::

   groups:
     - name: sakuli-alerts
       rules:
         - alert: SakuliTestFailure
           expr: sakuli_testcase_status > 0
           for: 1m
           labels:
             severity: critical
           annotations:
             summary: "Sakuli Test Case Failed"
             description: "Test case {{ $labels.testCase }} failed with status {{ $value }}"

Visualizing in Grafana
----------------------
- Import Prometheus as a data source in Grafana.
- Create dashboards for test case status, execution time, and error trends.
- Combine application-level monitoring with Sakuli E2E test results.

References
----------
- Sakuli Documentation: https://sakuli.io/docs
- Prometheus Documentation: https://prometheus.io/docs/
- Grafana Documentation: https://grafana.com/docs/
