---
title: Verileri Azure SQL Data warehouse'a Azure Data Factory kullanarak | Microsoft Docs
description: Azure SQL Data Warehouse'a veri kopyalamak için Azure Data Factory kullanın
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: c7549297f040e251f3c0109debf757c28750d0a0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34619278"
---
# <a name="load-data-into-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure SQL Data Warehouse'a veri yükleme

[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) büyük miktarda veriyi, ilişkisel ve ilişkisel olmayan işleyebilen bulut tabanlı, genişleme bir veritabanıdır. SQL veri ambarı kurumsal veri ambarı iş yükleri için en iyi duruma getirilmiş yüksek düzeyde paralel işleme (MPP) mimarisi üzerine inşa edilmiştir. Bu bulut esneklik depolama ölçeklendirmenizi ve bağımsız olarak işlem esnekliği sunar.

Azure SQL Data Warehouse ile çalışmaya başlama şimdi her zamankinden Azure Data Factory kullandığınızda daha kolay olur. Azure Data Factory, bulut tabanlı tam olarak yönetilen bir veri tümleştirme hizmetidir. Hizmet varolan sisteminizden SQL Data Warehouse verilerle doldurmak ve zamandan tasarruf için kullanabileceğiniz de analiz çözümleriniz oluştururken.

Azure Data Factory Azure SQL Data Warehouse'a veri yükleme için şu avantajları sağlar:

* **Ayarlamak kolay**: gerekli komut dosyası yok ile sezgisel bir 5-adım Sihirbazı.
* **Zengin veri depolamak Destek**: zengin bir şirket içi ve bulut tabanlı veri depoları için yerleşik destek. Tablonun ayrıntılı bir listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
* **Güvenli ve uyumlu**: veriler HTTPS veya ExpressRoute üzerinden aktarılır. Küresel hizmet varlığı verilerinizi coğrafi sınır asla terk sağlar.
* **PolyBase kullanarak benzersiz performansı**: Polybase, Azure SQL Data Warehouse'a veri taşımak için en verimli şekilde eder. Veri depoları, Azure Blob Depolama ve Data Lake Store dahil olmak üzere tüm türlerinden yüksek yük hızları elde etmek için hazırlama blob özelliğini kullanın. (Polybase Azure Blob Depolama ve Azure Data Lake Store varsayılan olarak destekler.) Ayrıntılar için bkz [kopyalama etkinliği performansının](copy-activity-performance.md).

Bu makalede, veri fabrikası kopya veri aracının nasıl kullanılacağı gösterilmektedir _verileri Azure SQL veritabanından Azure SQL Data warehouse'a_. Diğer veri depolarına türlerinden verileri kopyalamak için benzer adımları izleyebilirsiniz.

> [!NOTE]
> Daha fazla bilgi için bkz: [veri kopyalama ya da Azure SQL veri ambarından Azure Data Factory kullanarak](connector-azure-sql-data-warehouse.md).
>
> Bu makale, şu anda önizleme sürümünde olan Azure Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [kopya etkinliği Azure Data factory'de sürüm 1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği: Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) başlamadan önce.
* Azure SQL Data Warehouse: Veri ambarı üzerinde SQL veritabanından kopyalanan veriler tutar. Azure SQL Data Warehouse yoksa deki yönergelere bakın [SQL Data Warehouse oluşturma](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md).
* Azure SQL veritabanı: Bu öğretici, Adventure Works LT örnek verilerle bir Azure SQL veritabanından veri kopyalar. Bir SQL veritabanı'ndaki yönergeleri izleyerek oluşturabileceğiniz [bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md). 
* Azure depolama hesabı: Azure depolama alanı olarak kullanılan _hazırlama_ blob toplu kopyalama işleminde. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account) bölümündeki yönergelere bakın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüden seçin **yeni** > **veri + analiz** > **Data Factory**: 
   
   ![Yeni bir veri fabrikası oluşturma](./media/load-azure-sql-data-warehouse/new-azure-data-factory-menu.png)
2. İçinde **yeni data factory** sayfasında, aşağıdaki görüntüde gösterilen alanlar için değerleri girin:
      
   ![Yeni veri fabrikası sayfası](./media/load-azure-sql-data-warehouse/new-azure-data-factory.png)
 
    * **Ad**: Azure data factory'nizi için genel benzersiz bir ad girin. Hata alırsanız, "veri fabrikası adı \"LoadSQLDWDemo\" kullanılamıyor" veri fabrikası için farklı bir ad girin. Örneğin, adı kullanabilirsiniz  _**adınız**_**ADFTutorialDataFactory**. Data factory oluşturmayı yeniden deneyin. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: data factory oluşturulacağı Azure aboneliğinizi seçin. 
    * **Kaynak grubu**: aşağı açılan listeden mevcut bir kaynak grubu seçin ya da seçin **Yeni Oluştur** seçeneği ve bir kaynak grubu adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
    * **Sürüm**: seçin **V2 (Önizleme)**.
    * **Konum**: data factory konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depolarına diğer konumları ve bölgelerde olabilir. Bu veri depolarına Azure Data Lake Store, Azure Storage, Azure SQL Database vb. içerir.

3. **Oluştur**’u seçin.
4. Oluşturma işlemi tamamlandıktan sonra veri fabrikanıza gidin. Gördüğünüz **Data Factory** aşağıdaki görüntüde gösterildiği gibi giriş sayfası:
   
   ![Data factory giriş sayfası](./media/load-azure-sql-data-warehouse/data-factory-home-page.png)

   Seçin **Yazar & İzleyici** ayrı bir sekmede veri tümleştirme uygulamayı başlatmak için döşeme.

## <a name="load-data-into-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı’na veri yükleme

1. İçinde **başlama** sayfasında, **veri Kopyala** döşeme veri Kopyala aracını başlatmak için:

   ![Veri Kopyalama aracının kutucuğu](./media/load-azure-sql-data-warehouse/copy-data-tool-tile.png)
2. İçinde **özellikleri** sayfasında, belirtin **CopyFromSQLToSQLDW** için **görev adı** alan ve select **sonraki**:

    ![Özellikler sayfası](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)
3. İçinde **kaynak veri deposu** sayfasında, **Azure SQL veritabanı**seçip **sonraki**:

    ![Kaynak veri deposu sayfası](./media/load-azure-sql-data-warehouse/specify-source.png)
4. **Azure SQL veritabanını belirtin** sayfasında aşağıdaki adımları uygulayın: 
   1. **Sunucu adı** için Azure SQL Server’ınızı seçin.
   2. **Veritabanı adı** için Azure SQL veritabanınızı seçin.
   3. **Kullanıcı adı** için kullanıcının adını seçin.
   4. Kullanıcının parolasını belirtin **parola**.
   5. **İleri**’yi seçin.
   
   ![Azure SQL DB belirtin](./media/load-azure-sql-data-warehouse/specify-source-connection.png)
5. İçinde **veri kopyalama veya özel bir sorgu kullanmak üzere tabloları seçme** want **SalesLT** tabloları filtre uygulamak için. Seçin **(Tümünü Seç)** tüm tablolar için kopya kullanmayı kutusuna ve ardından **sonraki**: 

    ![Kaynak tabloları seçin](./media/load-azure-sql-data-warehouse/select-source-tables.png)

6. İçinde **hedef veri deposu** sayfasında, **Azure SQL Data Warehouse**seçip **sonraki**:

    ![Hedef veri deposu sayfası](./media/load-azure-sql-data-warehouse/specify-sink.png)
7. İçinde **Azure SQL Data Warehouse belirtin** sayfasında, aşağıdaki adımları uygulayın: 

   1. **Sunucu adı** için Azure SQL Server’ınızı seçin.
   2. Azure SQL Data Warehouse için seçin **veritabanı adı**.
   3. **Kullanıcı adı** için kullanıcının adını seçin.
   4. Kullanıcının parolasını belirtin **parola**.
   5. **İleri**’yi seçin.
   
   ![Azure SQL veri ambarı belirtin](./media/load-azure-sql-data-warehouse/specify-sink-connection.png)
8. İçinde **tablo eşlemesi** sayfasında içeriğini gözden geçirin ve seçin **sonraki**. Bir akıllı tablo eşlemesi görüntüler. Kaynak tabloları Tablo adlarına göre hedef tablolar eşlenir. Bir kaynak tablo hedef yoksa, Azure Data Factory varsayılan olarak aynı ada sahip bir hedef tablo oluşturur. Bu gibi durumlarda, bir kaynak tablo aynı zamanda var olan bir hedef tabloya eşleyebilirsiniz. 

   > [!NOTE]
   > SQL Server veya Azure SQL veritabanı kaynak olduğunda Otomatik Tablo oluşturma SQL Data Warehouse havuz için geçerlidir. Başka bir kaynak veri deposundan verileri kopyalarsanız, Azure SQL Data Warehouse havuz şemada veri kopyalamayı yürütmeden önce önceden oluşturmanız gerekir.

   ![Tablo eşleme sayfası](./media/load-azure-sql-data-warehouse/specify-table-mapping.png)

9. İçinde **şema eşleme** sayfasında içeriğini gözden geçirin ve seçin **sonraki**. Akıllı tablo eşlemesi sütun adına temel alır. Veri tabloları otomatik olarak oluşturma fabrikası izin vermek istiyorsanız, veri türü dönüşümü kaynak ve hedef depoları arasında uyumsuzluk olduğunda oluşabilir. Kaynak ve hedef sütun arasında bir desteklenmeyen veri türü dönüştürme ise, ilgili tablo yanındaki hata iletisine bakın.

    ![Şema eşleme sayfası](./media/load-azure-sql-data-warehouse/specify-schema-mapping.png)

11. İçinde **ayarları** sayfasında, Azure depolama hesabı seçin **depolama hesabı adı** aşağı açılan liste. Hesap PolyBase kullanarak SQL Data Warehouse'a yüklenmeden önce verileri hazırlamak için kullanılır. Kopyalama tamamlandıktan sonra geçici verileri Azure depolama alanında otomatik olarak temizlenir. Altında **Gelişmiş ayarları**, seçimini **türü Varsayılanı kullan** seçeneği:

    ![Ayarlar sayfası](./media/load-azure-sql-data-warehouse/copy-settings.png)
12. İçinde **Özet** sayfasında, ayarları gözden geçirin ve seçin **sonraki**:

    ![Özet sayfası](./media/load-azure-sql-data-warehouse/summary-page.png)
13. İçinde **dağıtım sayfası**seçin **İzleyici** (görev) işlem hattını izleme:

    ![Dağıtım sayfası](./media/load-azure-sql-data-warehouse/deployment-page.png)
14. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemler** sütununu içeren bağlantılar Etkinlik ayrıntıları görüntülemek ve ardışık düzeni yeniden çalıştırmak için: 

    ![İşlem hattı çalıştırmalarını izleme](./media/load-azure-sql-data-warehouse/monitor-pipeline-run.png)
15. Çalıştırma ardışık düzen ile ilişkili etkinlik çalışması görüntülemek için seçin **etkinlik görünümü çalışır** bağlamak **Eylemler** sütun. Ardışık düzeninde 10 kopyalama etkinlikleri vardır ve her etkinlik tek bir veri tablosu kopyalar. Ardışık Düzen dönmeyi görünüm çalışır, seçin **ardışık düzen** üst bağlantı. Listeyi yenilemek için **Yenile**’yi seçin. 

    ![Etkinlik çalıştırmalarını izleme](./media/load-azure-sql-data-warehouse/monitor-activity-run.png)

16. Her kopyalama etkinliği yürütme ayrıntılarını görüntülemek için seçin **ayrıntıları** altında bağlantı **Eylemler** izleme görünümü etkinliğinde. Veri birimi kaynak sunucudan kopyaladığınız havuz, veri işleme, yürütme adımları karşılık gelen süresiyle ve yapılandırmaları kullanılan gibi ayrıntıları izleyebilir:

    ![Ayrıntılar Çalıştır etkinliğini izleme](./media/load-azure-sql-data-warehouse/monitor-activity-run-details.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure SQL Data Warehouse desteği hakkında bilgi edinmek için aşağıdaki makaleyi ilerletmek: 

> [!div class="nextstepaction"]
>[Azure SQL Data Warehouse Bağlayıcısı](connector-azure-sql-data-warehouse.md)
