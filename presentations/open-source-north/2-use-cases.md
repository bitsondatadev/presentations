# Use cases

-vertical

## Case 1: SQL on anything

<div style="float: left; width: 60%; text-align: left; font-size:24px;">
  <ul>
    <li>Trino speaks the language of many data sources</li>
    <li>This is thanks to the Server Provider Interface (SPI)</li>
    <li>Data sources include databases, REST services, CSV files, or any data that can be represented in a tabular format</li>
  </ul>
</div>


<div style="float: left; width: 40%; ">
  <lottie-player src="../../assets/animations/sql-on-anything.json" background="transparent"  speed="1"  style="width: 25vw; display: block; margin-left: auto; margin-right: auto;" loop controls autoplay></lottie-player>
</div>

-vertical

## Case 2: Query federation

<div style="float: left; width: 60%; text-align: left; font-size:24px;">
  <ul>
    <li>All data is accessible from one terminal as opposed to moving data into one location before you can query it</li>
    <li>Unlike other data virtualization that relies solely on pushdown, Trino streams data across the network to process data on its own infrastructure</li>
  </ul>
</div>



<lottie-player src="../../assets/animations/distributed-federated.json" background="transparent"  speed="1"  style="width: 20vw; display: block; margin-left: auto; margin-right: auto;" loop controls autoplay></lottie-player>

<!-- .element style="float: left; width: 40%;" -->

-vertical

## Case 3: Fast object storage analytics

* Object storage is cheap, distributed, and commonly used to store open
  filetypes such as ORC, Parquet, JSON, and CSV files
* Data lake is the common term for storing data in unstructured formats
* Earlier data lakes were slow due to runtime, ineffecient data representation,
  and lack of accessibility
* New technologies improved upon the speed and discoverability of data such as
  Iceberg, Amundsen, and Trino
* Trino enabled adhoc queries over large data lakes with petabytes of data
* Data Lakehouse is a Data Lake including scheduled performance maintenance,
  data governance, and improved data cataloging

