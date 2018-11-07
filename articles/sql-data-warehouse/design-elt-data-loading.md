---
title: ETL yerine Azure SQL veri ambarı için ELT tasarlama | Microsoft Docs
description: ETL yerine, verileri veya Azure SQL veri ambarı'nı yüklemek için ayıklama, yükleme ve dönüştürme (ELT) işlemi tasarlayın.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: d004ad1f24448da0c7404761ca0865826b3000b3
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261290"
---
# <a name="designing-extract-load-and-transform-elt-for-azure-sql-data-warehouse"></a>Ayıklama, yükleme ve dönüştürme (ELT) Azure SQL veri ambarı için tasarlama

Ayıklama, dönüştürme ve yükleme (ETL) yerine, Azure SQL Data Warehouse'a veri yükleme için ayıklama, yükleme ve dönüştürme (ELT) işlemi tasarlayın. Bu makalede bir Azure veri ambarı'na veri taşıyan bir ELT işlem tasarım yollarını açıklar.

> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]

## <a name="what-is-elt"></a>ELT nedir?

Ayıklama, yükleme ve dönüştürme (ELT) veri kaynak sistemden bir hedef veri ambarı'na taşıyan bir işlemdir. Bu işlemi düzenli olarak, örneğin saat başı gerçekleştirilir veya günlük, yeni almak için veri ambarı'na veri oluşturulabilir. Veri ambarına veri kaynağından veri almak için ideal bir şekilde SQL veri ambarı'na veri yüklemek için PolyBase kullanan bir ELT işlem geliştirmektir.

ELT ilk yükler ve verileri ayıklama, dönüştürme ve yükleme (ETL) dönüştüren ise verileri yüklemeden önce ardından dönüştürür. ETL yerine ELT gerçekleştirme, yüklenmeden önce verileri dönüştürmek için kendi kaynakları sağlama maliyeti kaydeder. SQL veri ambarı kullanırken ELT dönüştürmeler gerçekleştirmek için MPP sistem yararlanır.

SQL veri ambarı için ELT uygulamak için birçok farklılıklar olsa da, temel adımlar şunlardır:  

1. Kaynak verileri metin dosyalarına ayıklayın.
2. Verileri Azure Blob Depolama veya Azure Data Lake Store gelirsiniz.
3. Veri yüklemesi için hazırlayın.
2. Hazırlama tablolarından PolyBase kullanarak SQL veri ambarı'na veri yükleme.
3. Verileri dönüştürün.
4. Üretim tablolarına veri ekleyin.


Yükleme öğreticisi için bkz. [verileri Azure blob depolama alanından Azure SQL veri ambarı'na yüklemek için PolyBase kullanma](load-data-from-azure-blob-storage-using-polybase.md).

Daha fazla bilgi için [desenleri blog yüklenirken](https://blogs.msdn.microsoft.com/sqlcat/2017/05/17/azure-sql-data-warehouse-loading-patterns-and-strategies/). 

## <a name="options-for-loading-with-polybase"></a>PolyBase ile yükleme seçenekleri

PolyBase, T-SQL dili ile veritabanı dışında verilere erişen bir teknolojidir. SQL Data Warehouse'a veri yükleme için en iyi yoludur. PolyBase ile veri işlem düğümleri için doğrudan veri kaynağından paralel yükler. 

PolyBase ile veri yükleme için şu yükleme seçeneklerinden herhangi birini kullanabilirsiniz.

- [PolyBase ile T-SQL](load-data-from-azure-blob-storage-using-polybase.md) verilerinizi Azure Blob Depolama veya Azure Data Lake Store içinde olduğunda iyi çalışır. Yükleme işlemi üzerinde en çok denetimi verir, ancak Ayrıca, dış veri nesneleri tanımlamanızı gerektirir. Kaynak tablolarına hedef tablolara eşleme gibi diğer yöntemleri bu nesneleri Sahne arkasında tanımlayın.  T-SQL yükleri düzenlemek için Azure Data Factory, SSIS veya Azure işlevleri kullanabilirsiniz. 
- [PolyBase ile SSIS](/sql/integration-services/load-data-to-sql-data-warehouse) kaynak verilerinizi SQL Server'da SQL Server şirket içinde veya bulutta olduğunda iyi çalışır. SSIS kaynağı için hedef tablo eşlemelerini tanımlar ve ayrıca yük düzenler. SSIS paketlerini zaten varsa, yeni veri ambarı hedefi ile çalışmak için paketlerini değiştirebilirsiniz. 
- [PolyBase ile Azure Data Factory (ADF)](sql-data-warehouse-load-with-data-factory.md) başka bir düzenleme aracıdır.  Bu, bir işlem hattı tanımlar ve işleri zamanlar. 
- [PolyBase ile Azure DataBricks](../azure-databricks/databricks-extract-load-sql-data-warehouse.md) aktarır verileri SQL veri ambarı tablodan bir Databricks veri çerçevesi için ve/veya bir SQL veri ambarı tablosuna bir Databricks dataframe verileri yazar.

### <a name="polybase-external-file-formats"></a>PolyBase dış dosya biçimleri

PolyBase UTF-8'den verileri yükler ve UTF-16 kodlamalı sınırlandırılmış metin dosyaları. Sınırlandırılmış metin dosyalarının yanı sıra Hadoop dosyası biçimlerinden RC dosyası ORC ve Parquet yükler. PolyBase, Gzip ve Snappy sıkıştırılmış dosyalarından veri yükleyebilir. PolyBase, genişletilmiş ASCII, sabit genişlikli biçimi ve iç içe geçmiş biçimleri WinZip, JSON ve XML gibi şu anda desteklemiyor.

### <a name="non-polybase-loading-options"></a>Olmayan PolyBase yükleme seçenekleri
Verilerinizi PolyBase ile uyumlu değilse, kullanabileceğiniz [bcp](/sql/tools/bcp-utility) veya [SQLBulkCopy API](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx). BCP Azure Blob Depolama geçmeden doğrudan SQL veri ambarı'na yükler ve yalnızca küçük yükler için tasarlanmıştır. Unutmayın, bu seçeneklerin yükleme performansını önemli ölçüde PolyBase yavaştır. 


## <a name="extract-source-data"></a>Kaynak verileri ayıklama

Veri kaynağı sisteminizi dışında alma kaynağa bağlıdır.  Sınırlandırılmış metin dosyalarına verileri taşımak için hedeftir. SQL Server kullanıyorsanız, kullanabileceğiniz [bcp komut satırı aracını](/sql/tools/bcp-utility) verileri dışarı aktarmak için.  

## <a name="land-data-to-azure-storage"></a>Azure depolama veri Arazi

Verileri Azure depolamada kavuşmak için kendisine taşıyabilirsiniz [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md) veya [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md). İki konumdan birinde verileri metin dosyalarına depolanması gerekir. Polybase, her iki konumdan yükleyebilir.

Bunlar araçları ve Hizmetleri Azure Depolama'ya veri taşımak için kullanabilirsiniz.

- [Azure ExpressRoute](../expressroute/expressroute-introduction.md) hizmeti ağ aktarım hızı, performans ve tahmin edilebilirliğini artırır. ExpressRoute adanmış özel bağlantı üzerinden verilerinizi Azure'a yönlendiren bir hizmettir. ExpressRoute bağlantıları ortak internet üzerinden veri yönlendirmediğinden. Bağlantılar genel internet üzerinden daha fazla güvenilirlik, daha yüksek hız, düşük gecikme ve tipik bağlantılardan daha yüksek güvenlik sunar.
- [AZCopy yardımcı programını](../storage/common/storage-moving-data.md) verileri Azure Depolama'ya genel internet üzerinden taşır. Bu, veri boyutu 10 TB'den az olduğunda çalışır. AZCopy ile düzenli aralıklarla yükleri gerçekleştirmek için kabul edilebilir olup olmadığını görmek için ağ hızını test edin. 
- [Azure Data Factory (ADF)](../data-factory/introduction.md) sahip bir ağ geçidi, yerel sunucuya yükleyebilirsiniz. Ardından, Azure depolama kadar yerel sunucunuzdan veri taşımak için işlem hattı oluşturabilirsiniz. Data Factory, SQL veri ambarı ile kullanmak için bkz: [SQL Data Warehouse'a veri yükleme](/azure/data-factory/load-azure-sql-data-warehouse).

## <a name="prepare-data"></a>Verileri hazırlama

Hazırlamak ve SQL veri ambarı'na yüklemeden önce depolama hesabınızdaki verileri temizlemek gerekebilir. Veri hazırlama, verileri metin dosyalarına dışarı aktarma gibi veri kaynağında olsa veya Azure Depolama'da veri olduktan sonra gerçekleştirilebilir.  Mümkün olduğunca erken işleminde verilerle çalışmak kolaydır.  

### <a name="define-external-tables"></a>Dış tabloları tanımlama
Verileri yüklemeden önce veri ambarı'nda dış tablolar tanımlamanız gerekir. PolyBase, tanımlamak ve Azure Depolama'daki verilere erişmek için dış tabloları kullanır. Dış tablo normal bir tabloya benzerdir. İkisi arasındaki temel fark dışında veri ambarına depolanan veriler için dış tablo noktasıdır. 

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

## <a name="load-to-a-staging-table"></a>Hazırlama tablosuna yükleme
Veri ambarı'na veri almak için ilk yükleme verileri bir hazırlama tablosuna çalışır. Hazırlama tablosuna kullanarak, üretim tablolarla engellemeden hataları işleyebilir ve üretim tablosuna geri alma işlemleri çalıştırmayın. Hazırlama tablosuna ayrıca üretim tablolarına veri eklemeden önce dönüştürmeleri çalıştırmak için SQL veri ambarı kullanmanıza olanak sağlar.

T-SQL ile yüklemek için Çalıştır [CREATE TABLE AS SELECT (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) T-SQL deyimi. Bu komut, yeni bir tabloya bir select deyiminin sonuçları ekler. Deyimi bir dış tablodan seçim yaptığında, dış verileri içeri aktarır. 

Aşağıdaki örnekte, dahili Bir dış tablo tarihtir. Tüm satırları dbo olarak adlandırılan yeni bir tabloya içeri aktarılır. Tarih.

```sql
CREATE TABLE [dbo].[Date]
WITH
( 
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT * FROM [ext].[Date]
;
```

## <a name="transform-the-data"></a>Verileri dönüştürme
Veri hazırlama tablosunda olsa da, İş yükünüzün gerektirdiği dönüşümleri gerçekleştirir. Ardından verileri üretim tablosuna taşıyın.

## <a name="insert-data-into-production-table"></a>Üretim tablosuna veri ekleme

İÇİNE ekle... SELECT deyimi veri hazırlama tablodan kalıcı tablosuna taşır. 

ETL işlemi tasarlarken, bir küçük test örneğinde çalışan işlem deneyin. 1000 satırı tablodan bir dosyaya ayıklama deneyin, Azure'a taşıyın ve sonra bir hazırlama tablosuna Yükleme'ı deneyin. 

## <a name="partner-loading-solutions"></a>İş ortağı çözümler yüklenirken
İş Ortaklarımızın çoğu, çözümleri yükleniyor vardır. Daha fazla bilgi için bir listesini görmek bizim [çözüm ortakları](sql-data-warehouse-partner-business-intelligence.md). 

## <a name="next-steps"></a>Sonraki adımlar
Yüklemeyle ilgili Rehber için bkz: [veri Yükleme Kılavuzu](guidance-for-loading-data.md).


