---
title: Veri yükleme Azure SQL veri ambarı'na Azure Data Factory kullanarak | Microsoft Docs
description: Azure SQL veri ambarı'na veri kopyalamak için Azure Data factory'yi kullanın.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: jingwang
ms.openlocfilehash: 6a7e0a27d3cda4193a04467d541f851a9e57fa46
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60549084"
---
# <a name="load-data-into-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure SQL Data Warehouse'a veri yükleme

[Azure SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) veriler, ilişkisel ve ilişkisel olmayan çok geniş hacimlerdeki işleyebilir bulut tabanlı, ölçeği genişletilmiş bir veritabanıdır. SQL veri ambarı, kurumsal veri ambarı iş yükleriniz için en iyi duruma getirilmiş yüksek düzeyde paralel işleme (MPP) mimarisi temel alınarak oluşturulmuştur. Bulut esnekliği bağımsız olarak işlem ve depolama ölçeklendirme yapma esnekliği sunar.

Azure SQL veri ambarı ile çalışmaya başlama artık hiç olmadığı kadar Azure Data Factory kullandığınızda daha kolaydır. Azure Data Factory, tam olarak yönetilen bulut tabanlı veri tümleştirme hizmetidir. Hizmet, mevcut sisteminizden SQL veri ambarı ile veri doldurun ve zamandan tasarruf için kullanabileceğiniz analiz çözümlerinizi oluştururken.

Azure Data Factory, Azure SQL Data Warehouse'a veri yükleme için aşağıdaki faydaları sağlar:

* **Kolay ayarlama**: Hiçbir gerekli komut ile bir sezgisel 5 adımlı Sihirbazı.
* **Zengin veri deposu desteği**: Zengin bir şirket içi ve bulut tabanlı veri depoları için yerleşik destek. Tablo ayrıntılı bir listesi için bkz [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
* **Güvenli ve uyumlu**: Veriler, HTTPS veya ExpressRoute üzerinden aktarılır. Küresel hizmet varlığını verilerinizi coğrafi sınır hiçbir zaman ayrılmaz sağlar.
* **PolyBase kullanarak benzersiz bir performansın**: Polybase, verileri Azure SQL Data Warehouse'a veri taşımak için en verimli yoludur. Veri depolarına, Azure Blob Depolama ve Data Lake Store dahil olmak üzere her türden yüksek yük hızlarını elde etmek için hazırlama blob özelliğini kullanın. (Polybase Azure Blob Depolama ve Azure Data Lake Store varsayılan olarak destekler.) Ayrıntılar için bkz [kopyalama etkinliği performansı](copy-activity-performance.md).

Bu makale, Data Factory-veri kopyalama aracını işlemini göstermektedir _yük verileri Azure SQL veritabanından Azure SQL veri ambarı'na_. Diğer türlerden veri depolarının veri kopyalamak için benzer adımları izleyebilirsiniz.

> [!NOTE]
> Daha fazla bilgi için [veri kopyalama için veya Azure SQL veri ambarı'ndan Azure Data Factory kullanarak](connector-azure-sql-data-warehouse.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Azure SQL veri ambarı: Veri ambarı üzerinde SQL veritabanından kopyalanan verileri tutar. Bir Azure SQL veri ambarı yoksa, yönergelere bakın [SQL veri ambarı oluşturma](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md).
* Azure SQL Veritabanı: Bu öğretici, Adventure Works LT örnek verileriyle bir Azure SQL veritabanından veri kopyalar. SQL veritabanı'ndaki yönergeleri takip ederek oluşturabileceğiniz [bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md). 
* Azure Storage hesabı: Azure depolama olarak kullanılan _hazırlama_ blob toplu kopyalama işleminde. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md) bölümündeki yönergelere bakın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüden **kaynak Oluştur** > **veri ve analiz** > **Data Factory**: 
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. İçinde **yeni veri fabrikası** sayfasında, aşağıdaki görüntüde gösterilen alanlar için değerleri sağlayın:
      
   ![Yeni veri fabrikası sayfası](./media/load-azure-sql-data-warehouse/new-azure-data-factory.png)
 
    * **Ad**: Azure data factory'nizi için genel olarak benzersiz bir ad girin. Hatasını alırsanız "veri fabrikası adı \"LoadSQLDWDemo\" kullanılabilir değil," veri fabrikası için farklı bir ad girin. Örneğin, adı kullanabilirsiniz  _**adınız**_**ADFTutorialDataFactory**. Veri Fabrikası oluşturmayı yeniden deneyin. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: Veri fabrikasının oluşturulacağı Azure aboneliğini seçin. 
    * **Kaynak grubu**: Aşağı açılan listeden mevcut bir kaynak grubunu seçin ya da seçin **Yeni Oluştur** seçenek ve bir kaynak grubu adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
    * **Sürüm**: Seçin **V2**.
    * **Konum**: Veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları başka konumlarda ve bölgelerde olabilir. Bu veri depolarına, Azure Data Lake Store, Azure depolama, Azure SQL veritabanı vb. içerir.

3. **Oluştur**’u seçin.
4. Oluşturma işlemi tamamlandıktan sonra veri fabrikanıza gidin. Gördüğünüz **Data Factory** aşağıdaki görüntüde gösterildiği gibi bir giriş sayfası:
   
   ![Data factory giriş sayfası](./media/load-azure-sql-data-warehouse/data-factory-home-page.png)

   Seçin **yazar ve İzleyici** veri tümleştirme uygulaması ayrı bir sekmede başlatmak için.

## <a name="load-data-into-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı’na veri yükleme

1. İçinde **başlama** sayfasında **veri kopyalama** veri kopyalama aracını başlatmak için:

   ![Veri Kopyalama aracının kutucuğu](./media/load-azure-sql-data-warehouse/copy-data-tool-tile.png)
1. İçinde **özellikleri** sayfasında, belirtin **CopyFromSQLToSQLDW** için **görev adı** alan ve seçim **sonraki**:

    ![Özellikler sayfası](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)

1. İçinde **kaynak veri deposu** sayfasında, aşağıdaki adımları tamamlayın:

    a. tıklayın **+ yeni bağlantı oluştur**:

    ![Kaynak veri deposu sayfası](./media/load-azure-sql-data-warehouse/new-source-linked-service.png)

    b. Seçin **Azure SQL veritabanı** galeri ve select **devam**. Bağlayıcılar filtrelemek için arama kutusuna "SQL" yazabilirsiniz.

    ![Azure SQL veritabanını seçme](./media/load-azure-sql-data-warehouse/select-azure-sql-db-source.png)

    c. İçinde **yeni bağlı hizmet** sayfasında sunucu adını ve veritabanı adı, açılır listeden seçin ve kullanıcı adı ve parola belirtin. Tıklayın **Bağlantıyı Sına** ayarlarını doğrulamak için seçip **son**.
   
    ![Azure SQL veritabanını yapılandırma](./media/load-azure-sql-data-warehouse/configure-azure-sql-db.png)

    d. Kaynak olarak yeni oluşturulan bağlantılı hizmeti seçin ve **İleri**'ye tıklayın.

    ![Kaynak olarak bağlantılı hizmeti seçin](./media/load-azure-sql-data-warehouse/select-source-linked-service.png)

1. İçinde **veri kopyalama veya özel sorgu kullanın tabloları seçin** want **SalesLT** tablolarını filtrelemek için. Seçin **(Tümünü Seç)** tüm tabloları kopyalama için kullanılacak kutusuna ve ardından **sonraki**: 

    ![Kaynak tabloları seçin](./media/load-azure-sql-data-warehouse/select-source-tables.png)

1. İçinde **hedef veri deposuna** sayfasında, aşağıdaki adımları tamamlayın:

    a. Bağlantı eklemek için **+ Yeni bağlantı oluştur**'a tıklayın

    ![Havuz veri deposu sayfası](./media/load-azure-sql-data-warehouse/new-sink-linked-service.png)

    b. Seçin **Azure SQL veri ambarı** galeri ve select **sonraki**.

    ![Azure SQL DW seçin](./media/load-azure-sql-data-warehouse/select-azure-sql-dw-sink.png)

    c. İçinde **yeni bağlı hizmet** sayfasında sunucu adını ve veritabanı adı, açılır listeden seçin ve kullanıcı adı ve parola belirtin. Tıklayın **Bağlantıyı Sına** ayarlarını doğrulamak için seçip **son**.
   
    ![Azure SQL DW yapılandırın](./media/load-azure-sql-data-warehouse/configure-azure-sql-dw.png)

    d. Kaynak olarak yeni oluşturulan bağlantılı havuz hizmetini seçin ve **İleri**'ye tıklayın.

    ![Havuza bağlı hizmeti seçin](./media/load-azure-sql-data-warehouse/select-sink-linked-service.png)

1. İçinde **Tablo eşleme** seçin sayfasında ve içeriği gözden geçir **sonraki**. Bir akıllı Tablo eşleme görüntüler. Kaynak tablolarına tablo adlarına göre hedef tablo eşlenir. Bir kaynak tablo hedefte yoksa, Azure Data Factory varsayılan olarak aynı ada sahip bir hedef tablo oluşturur. Ayrıca, bir kaynak tablo var olan bir hedef tabloya da eşleştirebilirsiniz. 

   > [!NOTE]
   > Otomatik Tablo oluşturma için SQL veri ambarı havuz, SQL Server veya Azure SQL veritabanı kaynak olduğunda geçerlidir. Başka bir kaynak veri deposundan verileri kopyalarsanız şemada havuz Azure SQL veri ambarı veri kopyalama yürütmeden önce önceden oluşturmanız gerekir.

   ![Tablo eşleme sayfası](./media/load-azure-sql-data-warehouse/table-mapping.png)

1. İçinde **şema eşleme** seçin sayfasında ve içeriği gözden geçir **sonraki**. Akıllı Tablo eşleme sütun adına bağlıdır. Data Factory otomatik olarak tablolar oluşturmak izin vermek istiyorsanız, veri türü dönüştürme kaynak ve hedef depoları arasında uyumsuzluk olduğunda oluşabilir. Kaynak ve hedef sütunu arasında bir desteklenmeyen veri türü dönüştürme varsa, karşılık gelen tabloya yanında bir hata iletisi görürsünüz.

    ![Şema eşleme sayfası](./media/load-azure-sql-data-warehouse/schema-mapping.png)

1. İçinde **ayarları** sayfasında, aşağıdaki adımları tamamlayın:

    a. İçinde **ayarları hazırlama** bölümünde **+ yeni** yeni hazırlama depolama. Depolama, PolyBase kullanarak SQL veri ambarı'na yüklenmeden önce verileri hazırlamak için kullanılır. Kopyalama tamamlandıktan sonra geçici verileri Azure Depolama'ya otomatik olarak temizlenir. 

    ![Hazırlama yapılandırın](./media/load-azure-sql-data-warehouse/configure-staging.png)

    b. İçinde **yeni bağlı hizmet** sayfasında, depolama hesabınızı seçin ve seçin **son**.
   
    ![Azure Storage yapılandırın](./media/load-azure-sql-data-warehouse/configure-blob-storage.png)

    c. İçinde **Gelişmiş ayarlar** bölümünde, seçimini **türü Varsayılanı kullan** seçeneğini ve ardından **sonraki**.

    ![PolyBase yapılandırın](./media/load-azure-sql-data-warehouse/configure-polybase.png)

1. İçinde **özeti** sayfasında, ayarları gözden geçirin ve seçin **sonraki**:

    ![Özet sayfası](./media/load-azure-sql-data-warehouse/summary-page.png)
1. İçinde **dağıtım sayfası**seçin **İzleyici** işlem hattını (görev) izlemek için:

    ![Dağıtım sayfası](./media/load-azure-sql-data-warehouse/deployment-page.png)
1. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemleri** sütununda etkinlik çalıştırması ayrıntılarını görüntüleme ve işlem hattını yeniden çalıştırma bağlantılarını içerir: 

    ![İşlem hattı çalıştırmalarını izleme](./media/load-azure-sql-data-warehouse/pipeline-monitoring.png)
1. İşlem hattı çalıştırması ile ilişkili etkinlik çalıştırmalarını görüntülemek için seçin **etkinlik çalıştırmalarını görüntüle** bağlantısını **eylemleri** sütun. İşlem hattı çalıştırmaları görünümüne dönmek için seçin **işlem hatları** üstündeki bağlantısı. Listeyi yenilemek için **Yenile**’yi seçin. 

    ![Etkinlik çalıştırmalarını izleme](./media/load-azure-sql-data-warehouse/activity-monitoring.png)

1. Her bir kopyalama etkinliği için yürütme ayrıntıları izlemek için **ayrıntıları** altında bağlantı **eylemleri** izleme görünümü etkinlik. Veri kaynağından kopyalanan havuz, veri aktarım hızı, yürütme adımları karşılık gelen süre ve yapılandırmaları kullanılan gibi ayrıntıları izleyebilirsiniz:

    ![İzleyici etkinlik çalışma ayrıntıları](./media/load-azure-sql-data-warehouse/monitor-activity-run-details.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure SQL veri ambarı desteği hakkında bilgi edinmek için şu makaleye geçin: 

> [!div class="nextstepaction"]
>[Azure SQL veri ambarı Bağlayıcısı](connector-azure-sql-data-warehouse.md)
