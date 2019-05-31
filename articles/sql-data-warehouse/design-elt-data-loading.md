---
title: ETL yerine Azure SQL veri ambarı için ELT tasarlama | Microsoft Docs
description: ETL yerine, verileri veya Azure SQL veri ambarı'nı yüklemek için ayıklama, yükleme ve dönüştürme (ELT) işlemi tasarlayın.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: load data
ms.date: 05/10/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: de5649498dddcec8c65f2cfca6dcb39fa20a9267
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242253"
---
# <a name="designing-a-polybase-data-loading-strategy-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için stratejisi yüklenirken PolyBase veri tasarlama

SMP veri ambarları geleneksel bir ayıklama, dönüştürme ve yükleme (ETL) işlemi, verileri yüklemek için kullanın. Azure SQL veri ambarı avantajı ölçeklenebilirlik ve esneklik işlem ve depolama kaynakları alan yüksek düzeyde paralel işleme (MPP) bir mimaridir. Bir ayıklama, yükleme ve dönüştürme (ELT) işlemi kullanılarak MPP avantajlarından yararlanın ve yükleme öncesinde verilerin dönüştürmek için gerekli kaynakları ortadan kaldırın. SQL veri ambarı Polybase olmayan seçenekleri BCP ve SQL BulkCopy API gibi dahil olmak üzere birçok yükleme yöntemleri desteklese de tarih yüklemenin en hızlı ve en ölçeklenebilir PolyBase yoludur.  PolyBase, T-SQL dili ile Azure Blob Depolama veya Azure Data Lake Store içinde depolanan dış verilere erişen bir teknolojidir.

> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]


## <a name="what-is-elt"></a>ELT nedir?

Ayıklama, yükleme ve dönüştürme (ELT) olarak veri kaynak sistemden ayıklanan yüklenen veri ambarı'na ve ardından dönüştürülmüş bir işlemdir. 

Bir SQL veri ambarı PolyBase ELT uygulamak için temel adımlar şunlardır:

1. Kaynak verileri metin dosyalarına ayıklayın.
2. Verileri Azure Blob Depolama veya Azure Data Lake Store gelirsiniz.
3. Veri yüklemesi için hazırlayın.
4. Veri hazırlama tablolarından PolyBase kullanarak SQL Data Warehouse'a veri yükleme. 
5. Verileri dönüştürün.
6. Üretim tablolarına veri ekleyin.


Yükleme öğreticisi için bkz. [verileri Azure blob depolama alanından Azure SQL veri ambarı'na yüklemek için PolyBase kullanma](load-data-from-azure-blob-storage-using-polybase.md).

Daha fazla bilgi için [desenleri blog yüklenirken](https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/). 


## <a name="1-extract-the-source-data-into-text-files"></a>1. Kaynak verileri metin dosyalarına ayıklayın

Kaynak sisteminiz dışında veri alma, depolama konumuna bağlıdır.  Sınırlandırılmış metin dosyaları desteklenen PolyBase verileri taşımak için hedeftir. 

### <a name="polybase-external-file-formats"></a>PolyBase dış dosya biçimleri

PolyBase UTF-8'den verileri yükler ve UTF-16 kodlamalı sınırlandırılmış metin dosyaları. Sınırlandırılmış metin dosyalarının yanı sıra Hadoop dosyası biçimlerinden RC dosyası ORC ve Parquet yükler. PolyBase Ayrıca, Gzip ve Snappy sıkıştırılmış dosyalar veri yükleyebilirsiniz. PolyBase, genişletilmiş ASCII, sabit genişlikli biçimi ve iç içe geçmiş biçimleri WinZip, JSON ve XML gibi şu anda desteklemiyor. SQL Server'dan dışarı aktarıyorsanız, kullanabileceğiniz [bcp komut satırı aracını](/sql/tools/bcp-utility) verileri sınırlandırılmış metin dosyalarına veri aktarmak için. SQL DW veri türü eşlemesi Parquet aşağıda verilmiştir:

| **Parquet veri türü** |                      **SQL veri türü**                       |
| :-------------------: | :----------------------------------------------------------: |
|        tinyint        |                           tinyint                            |
|       smallint        |                           smallint                           |
|          int          |                             int                              |
|        bigint         |                            bigint                            |
|        boole        |                             bit                              |
|        double         |                            float                             |
|         float         |                             real                             |
|        double         |                            money                             |
|        double         |                          smallmoney                          |
|        string         |                            nchar                             |
|        string         |                           nvarchar                           |
|        string         |                             char                             |
|        string         |                           varchar                            |
|        binary         |                            binary                            |
|        binary         |                          varbinary                           |
|       timestamp       |                             date                             |
|       timestamp       |                        smalldatetime                         |
|       timestamp       |                          datetime2                           |
|       timestamp       |                           datetime                           |
|       timestamp       |                             time                             |
|       date        | İnt ve atama tarihi (1) yükleme </br> (2) [da Azure Databricks SQL DW bağlayıcı](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse#load-data-into-azure-sql-data-warehouse) ile </br> spark.conf.set( "spark.sql.parquet.writeLegacyFormat", "true" ) </br> (**yakında güncelleştirme**) |
|        decimal        | [Azure Databricks SQL DW bağlayıcı kullanma](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse#load-data-into-azure-sql-data-warehouse) ile </br> spark.conf.set( "spark.sql.parquet.writeLegacyFormat", "true" ) </br> (**yakında güncelleştirme**) |

## <a name="2-land-the-data-into-azure-blob-storage-or-azure-data-lake-store"></a>2. Verileri Azure Blob Depolama veya Azure Data Lake Store gelirsiniz.

Verileri Azure depolamada kavuşmak için kendisine taşıyabilirsiniz [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md) veya [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md). İki konumdan birinde verileri metin dosyalarına depolanması gerekir. PolyBase, her iki konumdan yükleyebilir.

Araçları ve Hizmetleri Azure Depolama'ya veri taşımak için kullanabilirsiniz:

- [Azure ExpressRoute](../expressroute/expressroute-introduction.md) hizmeti ağ aktarım hızı, performans ve tahmin edilebilirliğini artırır. ExpressRoute adanmış özel bağlantı üzerinden verilerinizi Azure'a yönlendiren bir hizmettir. ExpressRoute bağlantıları ortak internet üzerinden veri yönlendirmediğinden. Bağlantılar genel internet üzerinden daha fazla güvenilirlik, daha yüksek hız, düşük gecikme ve tipik bağlantılardan daha yüksek güvenlik sunar.
- [AZCopy yardımcı programını](../storage/common/storage-moving-data.md) verileri Azure Depolama'ya genel internet üzerinden taşır. Bu, veri boyutu 10 TB'den az olduğunda çalışır. AZCopy ile düzenli aralıklarla yükleri gerçekleştirmek için kabul edilebilir olup olmadığını görmek için ağ hızını test edin. 
- [Azure Data Factory (ADF)](../data-factory/introduction.md) sahip bir ağ geçidi, yerel sunucuya yükleyebilirsiniz. Ardından, Azure depolama kadar yerel sunucunuzdan veri taşımak için işlem hattı oluşturabilirsiniz. Data Factory, SQL veri ambarı ile kullanmak için bkz: [SQL Data Warehouse'a veri yükleme](/azure/data-factory/load-azure-sql-data-warehouse).


## <a name="3-prepare-the-data-for-loading"></a>3. Yükleme için verileri hazırlama

Hazırlamak ve SQL veri ambarı'na yüklemeden önce depolama hesabınızdaki verileri temizlemek gerekebilir. Veri hazırlama, verileri metin dosyalarına dışarı aktarma gibi veri kaynağında olsa veya Azure Depolama'da veri olduktan sonra gerçekleştirilebilir.  Mümkün olduğunca erken işleminde verilerle çalışmak kolaydır.  

### <a name="define-external-tables"></a>Dış tabloları tanımlama

Verileri yüklemeden önce veri ambarı'nda dış tablolar tanımlamanız gerekir. PolyBase, tanımlamak ve Azure Depolama'daki verilere erişmek için dış tabloları kullanır. Bir dış tablo, veritabanı görünümü benzerdir. Dış tablo tablo şemasını içerir ve veri ambarı dışında depolanan veriler için gösterir. 

Dış tablolar tanımlama, veri kaynağını, metin dosyalarını ve tablo tanımları biçimini belirten içerir. İhtiyacınız olacak T-SQL söz dizimi konularına şunlardır:
- [DIŞ VERİ KAYNAĞI OLUŞTURMA](/sql/t-sql/statements/create-external-data-source-transact-sql)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)
- [DIŞ TABLO OLUŞTURMA](/sql/t-sql/statements/create-external-table-transact-sql)

Dış nesneler oluşturma örneği için bkz: [dış tablolar oluşturma](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) yükleme öğreticide adım.

### <a name="format-text-files"></a>Biçim metin dosyaları

Dış nesneler tanımlandıktan sonra dış tablo ve dosya biçimi tanımını içeren metin dosyalarını satırlarını hizalamak gerekir. Metin dosyasının her bir satırdaki verileri tablo tanımı ile hizalamanız gerekir.
Metni biçimlendirmek için dosyalar:

- Verileriniz ilişkisel olmayan bir kaynaktan geliyorsa, satırlar ve sütunlar haline dönüştürmek gerekir. İlişkisel ve ilişkisel olmayan bir kaynaktan veri olup olmadığını, verileri veri yükleme planladığınız tablosunun sütun tanımları ile hizalamak için dönüştürülmesi gerekir. 
- SQL veri ambarı hedef tabloda sütun ve veri türleri ile hizalamak için metin dosyasındaki veri biçimi. Yanlış hizalanmış veri türleri dış metin dosyalarındaki ve veri ambarı tablosu arasında yük reddedilir satırları neden olur.
- Bir sonlandırıcı içeren metin dosyası ayrı alanlar.  Bir karakteri ya da kaynak verilerinizi bulunmayan bir karakter dizisi kullandığınızdan emin olun. İle belirtilen Sonlandırıcı kullanın [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql).


## <a name="4-load-the-data-into-sql-data-warehouse-staging-tables-using-polybase"></a>4. Veri hazırlama tablolarından PolyBase kullanarak SQL Data Warehouse'a veri yükleme

Verileri bir hazırlama tablosuna yüklemek için en iyi bir uygulamadır. Hazırlama tabloları, üretim tablolarla engellemeden hataları işlemek izin verin. Hazırlama tablosuna ayrıca SQL veri ambarı MPP veri dönüşümleri için üretim tablolarına veri eklemeden önce kullanmanıza olanak sağlar.

### <a name="options-for-loading-with-polybase"></a>PolyBase ile yükleme seçenekleri

PolyBase ile veri yükleme için şu yükleme seçeneklerinden herhangi birini kullanabilirsiniz:

- [PolyBase ile T-SQL](load-data-from-azure-blob-storage-using-polybase.md) verilerinizi Azure Blob Depolama veya Azure Data Lake Store içinde olduğunda iyi çalışır. Yükleme işlemi üzerinde en çok denetimi verir, ancak Ayrıca, dış veri nesneleri tanımlamanızı gerektirir. Kaynak tablolarına hedef tablolara eşleme gibi diğer yöntemleri bu nesneleri Sahne arkasında tanımlayın.  T-SQL yükleri düzenlemek için Azure Data Factory, SSIS veya Azure işlevleri kullanabilirsiniz. 
- [PolyBase ile SSIS](/sql/integration-services/load-data-to-sql-data-warehouse) kaynak verilerinizi SQL Server'da SQL Server şirket içinde veya bulutta olduğunda iyi çalışır. SSIS kaynağı için hedef tablo eşlemelerini tanımlar ve ayrıca yük düzenler. SSIS paketlerini zaten varsa, yeni veri ambarı hedefi ile çalışmak için paketlerini değiştirebilirsiniz. 
- [PolyBase ile Azure Data Factory (ADF)](sql-data-warehouse-load-with-data-factory.md) başka bir düzenleme aracıdır.  Bu, bir işlem hattı tanımlar ve işleri zamanlar. 
- [PolyBase ile Azure DataBricks](../azure-databricks/databricks-extract-load-sql-data-warehouse.md) aktarır verileri SQL veri ambarı tablodan bir Databricks veri çerçevesi için ve/veya veri bir Databricks dataframe PolyBase kullanarak SQL veri ambarı tablo için yazar.

### <a name="non-polybase-loading-options"></a>Olmayan PolyBase yükleme seçenekleri

Verilerinizi PolyBase ile uyumlu değilse, kullanabileceğiniz [bcp](/sql/tools/bcp-utility) veya [SQLBulkCopy API](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx). BCP Azure Blob Depolama geçmeden doğrudan SQL veri ambarı'na yükler ve yalnızca küçük yükler için tasarlanmıştır. Unutmayın, bu seçeneklerin yükleme performansını önemli ölçüde PolyBase yavaştır. 


## <a name="5-transform-the-data"></a>5. Verileri dönüştürme

Veri hazırlama tablosunda olsa da, İş yükünüzün gerektirdiği dönüşümleri gerçekleştirir. Ardından verileri üretim tablosuna taşıyın.


## <a name="6-insert-the-data-into-production-tables"></a>6. Üretim tablolarına veri ekleme

İÇİNE ekle... SELECT deyimi veri hazırlama tablodan kalıcı tablosuna taşır. 

ETL işlemi tasarlarken, bir küçük test örneğinde çalışan işlem deneyin. 1000 satırı tablodan bir dosyaya ayıklama deneyin, Azure'a taşıyın ve sonra bir hazırlama tablosuna Yükleme'ı deneyin. 


## <a name="partner-loading-solutions"></a>İş ortağı çözümler yüklenirken

İş Ortaklarımızın çoğu, çözümleri yükleniyor vardır. Daha fazla bilgi için bir listesini görmek bizim [çözüm ortakları](sql-data-warehouse-partner-business-intelligence.md). 


## <a name="next-steps"></a>Sonraki adımlar

Yüklemeyle ilgili Rehber için bkz: [veri Yükleme Kılavuzu](guidance-for-loading-data.md).


