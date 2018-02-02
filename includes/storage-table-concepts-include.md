## <a name="what-is-table-storage"></a>Tablo depolama nedir
Azure Tablo depolama, büyük miktarlarda yapısal veriyi depolar. Bu hizmet, Azure bulutunun içinden ve dışından gelen kimliği doğrulanmış çağrıları kabul eden bir NoSQL olmayan veri deposudur. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir. Tablo depolamanın yaygın kullanımları şunlardır:

* Web ölçekli uygulamalara hizmet verebilen yapılandırılmış verilerin TB depolaması
* Karmaşık katılımların, yabancı anahtarların veya depolanan yordamların gerekmediği ve hızlı erişim için normal olmayabilen veri kümelerinin depolanması
* Kümelenmiş dizin kullanarak hızlı veri sorgulaması
* WCF Veri Hizmeti .NET Kitaplıklarıyla OData protokolü ve LINQ sorgularını kullanarak verilere erişilmesi

Yapılandırılmış ve ilişkisel olmayan verilerin büyük kümelerini depolamak ve sorgulamak için Tablo depolamayı kullanabilirsiniz; tablolara talep arttıkça da ölçekleneceklerdir.

## <a name="table-storage-concepts"></a>Tablo depolama kavramları
Tablo depolamada şu bileşenler bulunur:

![Tablo depolama bileşen diyagramı][Table1]

* **URL biçimi:** Azure Table Storage hesapları bu biçimi kullanın:`http://<storage account>.table.core.windows.net/<table>`

  Azure Cosmos DB tablo API hesapları şu biçimi kullanın:`http://<storage account>.table.cosmosdb.azure.com/<table>`  

  Bu adres biçimini OData protokolüyle birlikte kullanarak doğrudan Azure tablolarını işaret edebilirsiniz. Daha fazla bilgi için bkz. [OData.org][OData.org].
* **Hesaplar:** tüm Azure Storage erişimi bir depolama hesabıyla yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../articles/storage/common/storage-scalability-targets.md). 

    Azure Cosmos DB tüm erişimi tablo API hesabı üzerinden yapılır. Bkz: [tablo API hesabı oluşturma](../articles/cosmos-db/create-table-dotnet.md#create-a-database-account) tablo API hesabı oluşturma Ayrıntılar için.
* **Tablo**: Tablo, bir varlıklar koleksiyonudur. Tablolar varlıklardaki şemayı zorlamaz; bu da, tek tabloda farklı özellik kümelerine sahip varlıklar olabileceği anlamına gelir.  
* **Varlık**: Varlık bir dizi özellikler kümesidir, veritabanı satırına benzer. Bir varlık olarak Azure Storage'da en çok 1 MB boyutunda olabilir. Bir varlık Azure Cosmos veritabanı 2 MB boyutunda olabilir.
* **Özellikler**: Özellik bir ad-değer çiftidir. Her varlıkta verileri depolayacak en çok 252 özellik olabilir. Her varlıkta ayrıca, bölüm anahtarını, satır anahtarını ve zaman damgasını belirten üç sistem özelliği bulunur. Aynı bölüm anahtarına sahip varlıklar daha hızlı sorgulanabilir ve atomik işlemlere eklenir/güncelleştirilir. Varlığın satır anahtarı bölüm içinde kendine ait benzersiz tanımlayıcıdır.

Tabloları ve özellikleri adlandırma hakkında daha fazla bilgi için bkz. [Tablo Hizmeti Veri Modelini Anlama](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).

[Table1]: ./media/storage-table-concepts-include/table1.png
[OData.org]: http://www.odata.org/
