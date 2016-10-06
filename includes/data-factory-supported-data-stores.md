Data Factory’deki Kopyalama Etkinliği bir kaynak veri deposundan havuz veri deposuna verileri kopyalar. Data Factory aşağıdaki veri depolarını destekler. Herhangi bir kaynaktan gelen veriler herhangi bir havuza yazılabilir. Bir depoya veya depodan veri kopyalama hakkında bilgi edinmek için veri deposuna tıklayın.

Kategori | Veri deposu | Kaynak olarak desteklenen | Havuz olarak desteklenen
:------- | :--------- | :------------------ | :-----------------
Azure | [Azure Blob depolama](../articles/data-factory/data-factory-azure-blob-connector.md) <br/> [Azure Data Lake Store](../articles/data-factory/data-factory-azure-datalake-connector.md) <br/> [Azure SQL Database](../articles/data-factory/data-factory-azure-sql-connector.md) <br/> [Azure SQL Veri Ambarı](../articles/data-factory/data-factory-azure-sql-data-warehouse-connector.md) <br/> [Azure Tablo depolama](../articles/data-factory/data-factory-azure-table-connector.md) <br/> [Azure DocumentDB](../articles/data-factory/data-factory-azure-documentdb-connector.md) <br/> | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓
Veritabanları | [SQL Server](../articles/data-factory/data-factory-sqlserver-connector.md)\* <br/> [Oracle](../articles/data-factory/data-factory-onprem-oracle-connector.md)\* <br/> [MySQL](../articles/data-factory/data-factory-onprem-mysql-connector.md)\* <br/> [DB2](../articles/data-factory/data-factory-onprem-db2-connector.md)\* <br/> [Teradata](../articles/data-factory/data-factory-onprem-teradata-connector.md)\* <br/> [PostgreSQL](../articles/data-factory/data-factory-onprem-postgresql-connector.md)\* <br/> [Sybase](../articles/data-factory/data-factory-onprem-sybase-connector.md)\* <br/>[Cassandra](../articles/data-factory/data-factory-onprem-cassandra-connector.md)\* <br/>[MongoDB](../articles/data-factory/data-factory-on-premises-mongodb-connector.md)\*<br/>[Amazon Redshift](../articles/data-factory/data-factory-amazon-redshift-connector.md) | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓<br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ | ✓ <br/> ✓ <br/> &nbsp; <br/> &nbsp; <br/> &nbsp; <br/> &nbsp;<br/> &nbsp;<br/> &nbsp;<br/> &nbsp; <br/>&nbsp;
Dosya | [Dosya Sistemi](../articles/data-factory/data-factory-onprem-file-system-connector.md)\* <br/> [HDFS](../articles/data-factory/data-factory-hdfs-connector.md)\* <br/> [Amazon S3](../articles/data-factory/data-factory-amazon-simple-storage-service-connector.md) | ✓ <br/> ✓ <br/> ✓ | ✓ <br/> &nbsp;<br/>&nbsp;
Diğer | [Salesforce](../articles/data-factory/data-factory-salesforce-connector.md)<br/> [Genel ODBC](../articles/data-factory/data-factory-odbc-connector.md)\* <br/> [Genel OData](../articles/data-factory/data-factory-odata-connector.md) <br/> [Web Tablosu (HTML tablosu)](../articles/data-factory/data-factory-web-table-connector.md) <br/> [GE Historian](../articles/data-factory/data-factory-odbc-connector.md#ge-historian-store)* | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓  | &nbsp; <br/> &nbsp; <br/> &nbsp; <br/> &nbsp;<br/> &nbsp;<br/> &nbsp;

> [AZURE.NOTE] * taşıyan veri depoları şirket içi veya Azure IaaS üzerinde olabilir ve bir şirket içi/Azure IaaS makinesine [Veri Yönetimi Ağ Geçidi](../articles/data-factory/data-factory-data-management-gateway.md) yüklemenizi gerektirir.




<!--HONumber=Sep16_HO4-->


