## Tablo Hizmeti nedir

Azure Table Storage hizmeti büyük miktarlarda yapısal veriyi depolar. Bu hizmet, Azure bulutunun içinden ve dışından gelen kimliği doğrulanmış çağrıları kabul eden bir NoSQL olmayan veri deposudur. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir. Tablo hizmetinin genel kullanımları şunları içerir:

-   Web ölçekli uygulamalara hizmet verebilen yapılandırılmış verilerin TB depolaması
-   Karmaşık katılımların, yabancı anahtarların veya depolanan yordamların gerekmediği ve hızlı erişim için normal olmayabilen veri kümelerinin depolanması
-   Kümelenmiş dizin kullanarak hızlı veri sorgulaması
-   WCF Veri Hizmeti .NET Kitaplıklarıyla OData protokolü ve LINQ sorgularını kullanarak verilere erişilmesi

Yapılandırılmış ve ilişkisel olmayan verilerin büyük kümelerini depolamak ve sorgulamak için Tablo hizmetini kullanabilirsiniz; tablolara talep arttıkça da ölçekleneceklerdir.

## Tablo Hizmeti Kavramları

Tablo hizmetinde şu bileşenler bulunur:

![Table1][Table1]

-   **URL biçimi:** Kod, buradaki adres biçimini kullanarak hesaptaki tabloları işaret eder:   
    http://`<storage account>`.table.core.windows.net/`<table>`  
      
    Bu adres biçimini OData protokolüyle birlikte kullanarak doğrudan Azure tablolarını işaret edebilirsiniz. Daha fazla bilgi için bkz. [OData.org][]

-   **Depolama Hesabı:** Tüm Azure Storage erişimi bir depolama hesabıyla yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](storage-scalability-targets.md).

-   **Tablo**: Tablo, bir varlıklar koleksiyonudur. Tablolar varlıklardaki şemayı zorlamaz; bu da, tek tabloda farklı özellik kümelerine sahip varlıklar olabileceği anlamına gelir. Depolama hesabında olabilecek tablo sayısı yalnızca depolama hesabının kapasite sınırıyla sınırlanır.

-   **Varlık**: Varlık bir dizi özellikler kümesidir, veritabanı satırına benzer. Varlığın boyutu en çok 1MB olabilir.

-   **Özellikler**: Özellik bir ad-değer çiftidir. Her varlıkta verileri depolayacak en çok 252 özellik olabilir. Her varlıkta ayrıca, bölüm anahtarını, satır anahtarını ve zaman damgasını belirten 3 sistem özelliği bulunur. Aynı bölüm anahtarına sahip varlıklar daha hızlı sorgulanabilir ve atomik işlemlere eklenir/güncelleştirilir. Varlığın satır anahtarı bölüm içinde kendine ait benzersiz tanımlayıcıdır.

Tabloları ve özellikleri adlandırma hakkında daha fazla bilgi için bkz. [Tablo Hizmeti Veri Modelini Anlama](https://msdn.microsoft.com/library/azure/dd179338.aspx).
  
  [Table1]: ./media/storage-table-concepts-include/table1.png
  [OData.org]: http://www.odata.org/



<!--HONumber=Jun16_HO2-->


