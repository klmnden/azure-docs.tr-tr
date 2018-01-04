---
title: "Azure SQL Data Warehouse için ELT tasarlama | Microsoft Docs"
description: "Azure'a veri taşıma ve Azure SQL Data Warehouse için ayıklama, yükleme ve dönüştürme (ELT) işlem tasarlamak için SQL veri ambarında verileri yüklenirken teknolojileri birleştirin."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 2253bf46-cf72-4de7-85ce-f267494d55fa
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 12/12/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: e94dca69c77c46034e318205279be5188e1371f5
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="designing-extract-load-and-transform-elt-for-azure-sql-data-warehouse"></a>Ayıklama, yükleme ve dönüştürme (ELT) Azure SQL veri ambarı için tasarlama

Giriş verileri Azure depolama ve Azure SQL Data Warehouse için ayıklama, yükleme ve dönüştürme (ELT) işlem tasarlamak için SQL veri ambarına verilerin yüklenmesi için teknolojileri birleştirin. Bu makalede Polybase ile yüklenmesini destekler ve Azure depolama biriminden SQL veri ambarında verileri yüklemek için T-SQL ile PolyBase kullanan bir ELT işlem tasarlama odaklanır teknolojiler sunar.

## <a name="what-is-elt"></a>ELT nedir?

, Yük, ayıklayın ve dönüştürme (ELT) veriler kaynak sistemden bir hedef veri ambarına taşınır bir işlemdir. Bu işlemi düzenli aralıklarla, örneğin saatlik gerçekleştirilir veya günlük, yeni almak için veri ambarına veri oluşturulabilir. Veri ambarına veri kaynağından veri almak için ideal SQL Data Warehouse'a veri yüklemek için PolyBase kullanan bir ELT işleme geliştirmek için yoludur.

ELT ilk yükler ve ayıklama, dönüştürme ve yükleme (ETL) dönüştürür ancak verileri yüklemeden önce veri dönüştürür. ETL yerine ELT gerçekleştirme onu yüklenmeden önce verileri dönüştürmek için kendi kaynakları sağlama maliyeti kaydeder. SQL veri ambarı kullanırken ELT MPP dönüştürmeleri gerçekleştirebilirsiniz sisteme yararlanır.

SQL Data Warehouse için ELT uygulamak için pek çok Çeşitleme olsa da, temel adımlar şunlardır:  

1. Veri kaynağını metin dosyalarına ayıklayın.
2. Verileri Azure Blob storage veya Azure Data Lake Store güden.
3. Veri yükleme için hazırlayın.
2. Veri hazırlama tablolarındaki PolyBase kullanarak SQL Data Warehouse yükleyin.
3. Verileri dönüştürmek.
4. Veriler üretim tablolarına ekleyin.


Bir yükleme öğretici için bkz: [kullanım verileri Azure blob depolama alanından Azure SQL Data Warehouse'a yüklemek için PolyBase](load-data-from-azure-blob-storage-using-polybase.md).

Daha fazla bilgi için bkz: [desenleri blog yüklenirken](http://blogs.msdn.microsoft.com/sqlcat/2017/05/17/azure-sql-data-warehouse-loading-patterns-and-strategies/). 

## <a name="options-for-loading-with-polybase"></a>PolyBase ile birlikte yükleme seçenekleri

PolyBase T-SQL dili ile veritabanı dışında verilere erişen bir teknolojidir. SQL Data Warehouse'a veri yükleme için en iyi yoludur. PolyBase ile veri işlem düğümleri için doğrudan veri kaynağından paralel yükler. 

PolyBase ile veri yüklemek için aşağıdaki yükleme seçeneklerinden herhangi birini kullanabilirsiniz.

- [T-SQL ile PolyBase](load-data-from-azure-blob-storage-using-polybase.md) verileriniz Azure Blob storage veya Azure Data Lake Store olduğunda iyi çalışır. Yükleme işlemi üzerinde çoğu denetim sunar, ancak Ayrıca, dış veri nesneleri tanımlamanızı gerektirir. Kaynak tablolar için hedef tablo eşlemesi olarak diğer yöntemleri arka planda bu nesneleri tanımlar.  T-SQL yükleri düzenlemek için Azure Data Factory, SSIS ya da Azure işlevlerini kullanabilirsiniz. 
- [SSIS Polybase'i](sql-data-warehouse-load-from-sql-server-with-integration-services.md) veri kaynağınızı SQL Server'daki ya da SQL Server şirket içi veya bulutta olduğunda iyi çalışır. SSIS kaynak hedef tablo eşlemelere tanımlar ve ayrıca yük yönetir. SSIS paketleri zaten varsa, yeni veri ambarı hedef çalışmak için paketlerini değiştirebilirsiniz. 
- [PolyBase Azure veri fabrikası (ADF) ile](sql-data-warehouse-load-with-data-factory.md) başka bir düzenleme aracıdır.  Bir işlem hattı tanımlar ve işleri zamanlar. 

### <a name="polybase-external-file-formats"></a>PolyBase dış dosya biçimleri

PolyBase UTF-8'den verileri yükler ve UTF-16 kodlu sınırlandırılmış metin dosyaları. Sınırlandırılmış metin dosyaları yanı sıra Hadoop dosya biçimlerinden RC dosya, ORC ve Parquet yükler. PolyBase, Gzip ve Snappy sıkıştırılmış dosyaların verileri yükleyebilirsiniz. PolyBase, genişletilmiş ASCII, sabit genişlik biçimine ve iç içe geçmiş biçimleri WinZip, JSON ve XML gibi şu anda desteklemiyor.

### <a name="non-polybase-loading-options"></a>Olmayan PolyBase yükleme seçenekleri
Verilerinizi PolyBase ile uyumlu değilse, kullanabileceğiniz [bcp](sql-data-warehouse-load-with-bcp.md) veya [SQLBulkCopy API](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx). BCP Azure Blob Depolama geçmeden doğrudan SQL Data Warehouse için yükler ve yalnızca küçük yükler için tasarlanmıştır. Bu seçenekler yükü performansını önemli ölçüde PolyBase yavaştır unutmayın. 


## <a name="extract-source-data"></a>Kaynak veri ayıklamak

Kaynak sisteminiz dışında veri alma kaynağa bağlıdır.  Sınırlandırılmış metin dosyalarına verileri taşımak için belirtilir. SQL Server kullanıyorsanız, kullanabileceğiniz [bcp komut satırı aracı](/sql/tools/bcp-utility) verilerini dışarı aktarmak için.  

## <a name="land-data-to-azure-storage"></a>Kara verileri Azure depolama

Verileri Azure depolama alanında güden için kendisine taşıyabilirsiniz [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md) veya [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md). Her iki konumda metin dosyalarına verilerin depolanması gerekir. Polybase ya da konumdan yükleyebilirsiniz.

Bunlar olan ve araçları ve Hizmetleri verileri Azure depolama birimine taşımak için kullanabilirsiniz.

- [Azure ExpressRoute](../expressroute/expressroute-introduction.md) hizmeti, ağ verimliliğini, performans ve öngörülebilirlik geliştirir. ExpressRoute, Azure için verilerinizi adanmış özel bağlantı üzerinden yönlendiren bir hizmettir. ExpressRoute bağlantıları ortak internet üzerinden veri yol değil. Bağlantıları ortak internet üzerinden daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantıları daha yüksek güvenlik sunar.
- [AZCopy yardımcı programı](../storage/common/storage-use-azcopy.md) verileri Azure depolama alanına genel internet üzerinden taşır. Bu, veri boyutu 10 TB değerinden küçük olduğunda çalışır. AZCopy ile düzenli olarak yükleri gerçekleştirmek için uygun olup olmadığını görmek için ağ hızı sınayın. 
- [Azure veri fabrikası (ADF)](../data-factory/introduction.md) yerel sunucunuzda yükleyebilmek için bir ağ geçidi sahiptir. Ardından, Azure Storage kadar yerel sunucunuzdan verileri taşımak için bir işlem hattı oluşturabilirsiniz.

Daha fazla bilgi için bkz: [için ve Azure Storage veri taşıma](../storage/common/storage-moving-data.md)


## <a name="prepare-data"></a>Verileri hazırlama

Hazırlama ve depolama hesabınızdaki veriler SQL Data Warehouse'a yüklemeden önce temizleme gerekebilir. Veri hazırlama metin dosyalarına veri aktarma gibi veri kaynağında aktarılırken ya da verileri Azure depolama alanında sonra gerçekleştirilebilir.  Mümkün olduğunca işleminde erken verilerle çalışmak daha kolaydır.  

### <a name="define-external-tables"></a>Dış tablolara tanımlayın
Verileri yüklemeden önce veri ambarında dış tablolara tanımlamanız gerekir. PolyBase tanımlamak ve Azure depolama alanındaki verilere erişmek için dış tabloları kullanır. Dış tablo normal bir tabloya benzer. Veri ambarı dışında depolanan veriler için dış tablo noktaları ana farktır. 

Dış tablolara tanımlama belirtilmesini veri kaynağı, metin dosyaları ve tablo tanımları biçimi içerir. İhtiyacınız olacak T-SQL söz dizimi konular şunlardır:
- [DIŞ VERİ KAYNAĞI OLUŞTURUN](/sql/t-sql/statements/create-external-data-source-transact-sql)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)
- [DIŞ TABLO OLUŞTURMA](/sql/t-sql/statements/create-external-table-transact-sql)

Dış nesne oluşturma örneği için bkz: [dış tablolar oluşturma](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) yükleme öğreticide adım.

### <a name="format-text-files"></a>Biçim metin dosyaları

Dış nesneleri tanımlandıktan sonra dış tablo ve dosya biçimi tanımı metin dosyalarını satırlarını uyumlu hale getirmeniz gerekir. Veriler, metin dosyasının her satır tablo tanımıyla hizalanması gerekir.

Metin dosyaları biçimlendirmek için:

- Verilerinizi ilişkisel olmayan bir kaynaktan geldiğinden, satırları ve sütunları dönüştürmesi gerekir. İlişkisel veya ilişkisel olmayan bir kaynaktan verileri olup olmadığını belirtir, verileri veri yükleme planladığınız tablosunun sütun tanımları hizalamak için dönüştürülmesi gerekir. 
- SQL veri ambarı hedef tabloda sütun ve veri türleriyle hizalamak için metin dosyasındaki veri biçimi. Dış metin dosyalarında veri türleri ve veri ambarı tablosu arasında uyuşmazlığın yüklenmesi sırasında reddedilmesi satırları neden olur.
- Metin dosyası bir sonlandırıcı ile ayrı alanlarda.  Bir karakter ya da kaynak verilerinizi bulunamadı bir karakter dizisi kullandığınızdan emin olun. Belirtilen ile Sonlandırıcı kullanmak [oluşturmak dış dosya BİÇİMİNİ](/sql/t-sql/statements/create-external-file-format-transact-sql).

## <a name="load-to-a-staging-table"></a>Hazırlama tabloya yükleyin
Veri ambarına veri almak için bu da ilk yük verileri hazırlama tabloya çalışır. Hazırlama tablosunda kullanarak üretim tablolarla engellemeden hataları işleyebilir ve geri alma tablosu üzerinde işlem üretim çalıştırmayın. Hazırlama tablosunu de üretim tablolara verileri eklemeden önce dönüşümleri çalıştırmak için SQL Data Warehouse kullanma olanağı sunar.

T-SQL ile yüklemek için Çalıştır [oluşturma tablo AS seçin (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md) T-SQL ifadesi. Bu komut bir select deyimi sonucunu yeni bir tabloya ekler. İfadesi bir dış tablosundan seçtiğinde, dış veri aktarır. 

Aşağıdaki örnekte, dahili Bir dış tablo tarihidir. Tüm satırları dbo olarak adlandırılan yeni bir tabloya içeri aktarılır. Tarih.

```sql
CREATE TABLE [dbo].[Date]
WITH
( 
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT * FROM [ext].[Date]
;
```

## <a name="transform-the-data"></a>Veri dönüştürme
Veri hazırlama tablosunda olsa da, İş yükünüzün gerektirdiği dönüştürmeleri gerçekleştirebilirsiniz. Ardından verileri üretim tabloya taşıyın.

## <a name="insert-data-into-production-table"></a>Üretim tabloya veri ekleme

INSERT INTO... SELECT deyimi veri kalıcı tabloya hazırlama tablosundan taşır. 

ETL işlemi tasarlarken, bir küçük test örneği üzerinde işlem çalıştırmayı deneyin. 1000 satırları tablodan bir dosyaya ayıklanıyor deneyin, Azure'a taşıyın ve sonra hazırlama tabloya yüklenirken deneyin. 

## <a name="partner-loading-solutions"></a>İş ortağı yükleme çözümü
Ortaklarımızın çoğunun çözümleri yüklenirken vardır. Daha fazla bilgi bulmak için bir listesini görmek bizim [çözüm ortakları](sql-data-warehouse-partner-business-intelligence.md). 

## <a name="next-steps"></a>Sonraki adımlar
Kılavuzu yüklemek için bkz: [veri yükleme kılavuzunu](guidance-for-loading-data.md).


