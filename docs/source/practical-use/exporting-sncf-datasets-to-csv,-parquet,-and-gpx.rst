Exporting SNCF Datasets to CSV, Parquet, and GPX: A Step-by-Step Guide
=======================================================================

Understand the export formats supported by the API, how to use custom parameters (like ``compressed``, ``with_bom``, ``epsg``), and when to choose each format for transport and GIS projects.

Overview
--------

The SNCF Open Data API allows you to export datasets in multiple formats including CSV, Parquet, and GPX.  
Each format has specific options and ideal use cases depending on your data processing workflow.

In this guide, you'll learn how to:

- Export data in CSV, Parquet, and GPX formats
- Customize the export with format-specific parameters
- Choose the right format based on your project type (e.g. data analysis or GIS)

Base URL
--------

::

    https://ressources.data.sncf.com/api/explore/v2.1/catalog

General Export Endpoint
------------------------

Use the following pattern to export a dataset:

::

    GET /datasets/{dataset_id}/exports/{format}

**Path Parameters:**

- ``dataset_id``: ID of the dataset to export (e.g. `gares`)
- ``format``: `csv`, `parquet`, or `gpx`

You can append query parameters to refine the export.

CSV Export
----------

Endpoint:

::

    GET /datasets/gares/exports/csv

**Optional CSV Parameters:**

- ``delimit``: Field delimiter (default is `,`; use `;` for European CSVs)
- ``list_separator``: Character to separate multiple values within a field (e.g., `|`)
- ``quote_all`` *(boolean)*: Whether to quote all fields
- ``with_bom`` *(boolean)*: Add BOM (Byte Order Mark) for Excel compatibility (default: `true` in v2.1)

**Example Request:**

::

    GET /datasets/gares/exports/csv?delimit=;&quote_all=true&with_bom=true

**Best use case:**  
CSV is ideal for spreadsheets, business analysts, and general-purpose tabular processing.

Parquet Export
--------------

Endpoint:

::

    GET /datasets/gares/exports/parquet

**Optional Parquet Parameter:**

- ``parquet_compression``: Compression codec, e.g., `snappy`, `gzip`

**Example Request:**

::

    GET /datasets/gares/exports/parquet?parquet_compression=snappy

**Best use case:**  
Parquet is great for big data processing, especially in tools like Apache Spark, Hadoop, or cloud platforms.

GPX Export
----------

Endpoint:

::

    GET /datasets/gares/exports/gpx

**Optional GPX Parameters:**

- ``name_field``: Field to use as the GPX waypoint name
- ``description_field_list``: Comma-separated list of fields to use in the description
- ``use_extension`` *(boolean)*: Whether to include the GPX `<extension>` tag (default: `true` in v2.1)

**Example Request:**

::

    GET /datasets/gares/exports/gpx?name_field=name&description_field_list=city,type&use_extension=true

**Best use case:**  
GPX is designed for geographic data and works well with GPS devices and mapping software like QGIS or Google Earth.

Tips and Notes
--------------

- All export endpoints support `where`, `select`, and `refine` filters just like the main `/records` endpoint.
- Combine filtering and export in one step to reduce file size and increase relevance.
- You can use the `compressed=true` parameter on most formats to get `.gz` files.

Choosing the Right Format
--------------------------

+------------+-----------------------------+-----------------------------+
| Format     | Use Case                    | Tools                       |
+============+=============================+=============================+
| CSV        | General analysis, Excel     | Excel, Python, R            |
+------------+-----------------------------+-----------------------------+
| Parquet    | Big data pipelines          | Spark, Pandas, Databricks   |
+------------+-----------------------------+-----------------------------+
| GPX        | Mapping, GPS routes         | QGIS, GPS devices, Leaflet  |
+------------+-----------------------------+-----------------------------+

Conclusion
----------

The SNCF Open Data API makes it easy to export datasets in flexible formats suitable for many types of projects.  
By leveraging the appropriate parameters, you can streamline your data handling for transportation planning, geographic visualizations, or machine learning pipelines.

References
----------

- API Console: https://ressources.data.sncf.com/api/explore/v2.1/console
- Official Documentation: https://docs.opendatasoft.com/en/
- SNCF Dataset Portal: https://ressources.data.sncf.com/pages/home/
