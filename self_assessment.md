<h1 align="center">Self-Assessment</h1>

<h2 align="center">Objectives Achieved</h1>

This MVP met the main objectives defined for the sprint, both from an academic perspective and as a professional portfolio piece.

- Implement an **end-to-end data pipeline in a cloud environment (Databricks)**, using Spark and Delta Lake for processing and storage. 
- Structure the **Bronze → Silver → Gold** layers with e-commerce and chargebacks data, following Lakehouse architecture and Data Engineering best practices. 
- Model the Gold layer in a **star schema**, with `fato_transacoes` and dimensions for customers, sellers, payments, date, geolocation and chargebacks. 
- Document the project for use as a **portfolio asset**, including README, ETL documentation, data catalog and this self-assessment. 

---

<h2 align="center">Challenges Encountered</h1>

During development, typical challenges of Data Engineering projects in a cloud environment arose, along with practical learning of new tools.

- **Learning, in practice, how to use Databricks as a cloud platform**  
  It was necessary to understand concepts of catalog, schemas, volumes, clusters and Delta Lake, going beyond the use of isolated notebooks to a more architectural use of the platform. 

- **Resource and environment limitations**  
  Environment constraints reduced the ability to test heavier performance scenarios, query tuning, experiments with larger volumes and more advanced cluster configurations. 

- **Sprint focus on batch loads**  
  Ingestion was implemented in batch mode, without exploring streaming or near real-time scenarios, even though these are highly relevant to the context of a payment gateway. 

- **Lack of full orchestration and automated data quality**  
  The pipeline is not yet integrated into a job pipeline with monitoring, logs and alerts, nor does it have a formal data quality framework with declarative rules and automatic reports per layer. 

---

<h2 align="center">Strengths</h1>

Even with the limitations, some points stand out positively within the specific scope of Data Engineering.

- **Clear, explainable layered architecture**  
  The Bronze/Silver/Gold split, combined with the use of Delta Lake and Unity Catalog, makes it easier to explain the pipeline to architects, engineers and technical recruiters. 

- **Well-defined analytical model (star schema)**  
  The existence of a central fact and thematic dimensions makes queries simpler, reduces join complexity and brings the project closer to the day-to-day reality of BI and Analytics teams. 

- **Lineage and organization by ETL stages**  
  Each script and layer has a clear responsibility (preparation, download, bronze, silver, gold), which helps with debugging, reprocessing and understanding data lineage. 

- **Portfolio-oriented documentation**  
  The focus on README, ETL documentation, catalog and self-assessment reinforces the ability to communicate architecture and technical decisions, which is essential in modern Data Engineering. 

---

<h2 align="center">Learnings</h1>

The development of this MVP brought specific learnings for working as a data engineer.

- **Thinking of data as a product**  
  Each layer and table was treated as a data product, with a clear target audience and use case, which directly influences schema, granularity and documentation decisions. 

- **Separation of responsibilities in the pipeline**  
  Organizing scripts by stage (preparation, ingestion, transformation, modeling) reinforced the importance of modular pipelines that are easier to evolve and orchestrate. 

- **Disciplined use of Spark and Lakehouse**  
  Practice reinforced the difference between local and distributed processing, and the importance of schema, types and governance (catalog, layers) to avoid future issues. 

---

<h2 align="center">Next Steps</h1>

As a natural evolution of this MVP, some next steps aligned with the Data Engineering role are:

- Migrate execution to an **orchestrated and monitored** flow, with jobs, dependencies and alerts. 
- Incorporate **streaming** to handle transactional events in near real-time. 
- Adopt a formal **data quality** layer with metrics, thresholds and recurring reports. 
- Perform **performance tuning** and cost analysis in scenarios with larger datasets and different cluster configurations. 

---