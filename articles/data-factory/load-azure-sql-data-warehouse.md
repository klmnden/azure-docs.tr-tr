---
title: Verileri Azure SQL Data warehouse'a Azure Data Factory kullanarak | Microsoft Docs
description: "Azure SQL Data Warehouse'a veri kopyalamak için Azure Data Factory kullanın"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: 36e24da50386d1abc441e2beb09f570a9612a346
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="load-data-into-azure-sql-data-warehouse-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure SQL veri ambarına veri yükleme

[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) bir bulut tabanlı genişleme büyük miktarda veriyi, ilişkisel ve ilişkisel olmayan işleyebilen veritabanıdır.  SQL Data Warehouse, yüksek düzeyde paralel işleme (MPP) mimarisi üzerine kurulu, kurumsal veri ambarı iş yükleri için optimize edilmiştir.  Bu bulut esneklik depolama ölçeklendirmenizi ve bağımsız olarak işlem esnekliği sunar.

Azure SQL Data Warehouse ile çalışmaya başlama artık her zamankinden kullanmaktan daha kolay **Azure Data Factory**.  Azure Data Factory SQL Data Warehouse, var olan sisteminizden ve SQL Data Warehouse değerlendirmek ve analizlerinizi oluşturma, değerli zamandan kaydetme verilerle doldurmak için kullanılan bir bulut tabanlı tam olarak yönetilen bir veri tümleştirme hizmetidir çözümleri. Azure SQL Data Azure Data Factory kullanarak Warehouse'a veri yükleme avantajları şunlardır:

* **Ayarlamak kolay**: 5-adım sezgisel Sihirbazı ile gerekli komut dosyası yok.
* **Zengin veri depolamak Destek**: zengin bir şirket içi ve bulut tabanlı veri depoları için yerleşik destek Bkz ayrıntılı listesinde [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.
* **Güvenli ve uyumlu**: veriler HTTPS veya ExpressRoute üzerinden aktarılır ve küresel hizmet varlığı sağlar, verilerinizin hiçbir zaman coğrafi sınır bırakır
* **PolyBase kullanarak benzersiz performansı**: Polybase kullanarak Azure SQL Data Warehouse'a veri taşımak için en verimli şekilde eder. Hazırlama blob özelliğini kullanarak, her türlü Azure Blob Depolama yanı sıra veri depoları ve varsayılan olarak Polybase destekler Data Lake Store yüksek yük hızlarını elde edebilirsiniz. Ayrıntıları öğrenmek [kopyalama etkinliği performansının](copy-activity-performance.md).

Bu makalede, veri fabrikası kopya veri aracının nasıl kullanılacağı gösterilmektedir **verileri Azure SQL veritabanından Azure SQL Data warehouse'a**. Diğer veri depolarına türlerinden verileri kopyalamak için benzer adımları izleyebilirsiniz.

> [!NOTE]
>  Veri kopyalama/Azure SQL Data Warehouse, genel veri fabrikasının özellikleri hakkında bilgi için bkz: [veri kopyalama ya da Azure SQL veri ambarından Azure Data Factory kullanarak](connector-azure-sql-data-warehouse.md) makalesi.
>
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Önkoşullar

* **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
* **Azure SQL Veri Ambarı**. Bu veri ambarı, SQL Veritabanından kopyalanan verileri tutar. Azure SQL Veri Ambarınız yoksa, oluşturma adımları için [Azure SQL Veri Ambarı oluşturma](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md) makalesine bakın.
* **Azure SQL Veritabanı**. Bu öğretici, Adventure Works LT örnek verilerle bir Azure SQL veritabanından veri kopyalar, bir aşağıdaki oluşturabilirsiniz [bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md) makalesi. 
* **Azure Depolama hesabı**. Azure depolama alanı olarak kullanılan **hazırlama** blob toplu kopyalama işleminde. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account) makalesine bakın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüde **YENİ**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/load-azure-sql-data-warehouse/new-azure-data-factory-menu.png)
2. İçinde **yeni data factory** sayfasında, aşağıdaki ekran görüntüsünde gösterildiği gibi değerlerini belirtin:
      
     ![Yeni veri fabrikası sayfası](./media/load-azure-sql-data-warehouse/new-azure-data-factory.png)
 
   * **Adı.** Veri Fabrikası için genel benzersiz bir ad girin. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin adınızADFTutorialDataFactory) ve oluşturmayı yeniden deneyin. Bkz: [Data Factory - adlandırma kurallarını](naming-rules.md) Data Factory yapıtlarının adlandırma kuralları için makale.
  
            `Data factory name "LoadSQLDWDemo" is not available`

    * **Aboneliği.** Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
    * **Kaynak grubu.** Aşağı açılan listeden mevcut bir kaynak grubu seçin ya da seçin **Yeni Oluştur** seçeneği ve bir kaynak grubu adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
    * **Version.** Seçin **V2 (Önizleme)**.
    * **Konum.** Veri Fabrikası için konum seçin. Yalnızca desteklenen konumlar aşağı açılan listede görüntülenir. Veri fabrikası tarafından kullanılan veri depoları (Azure Storage, Azure SQL Database, vb.), diğer konumları/bölgelerde olabilir. 

3. **Oluştur**’a tıklayın.
4. Oluşturma tam, veri fabrikası gidin ve gördüğünüz sonra **Data Factory** görüntüde gösterildiği gibi sayfa. Tıklatın **Yazar & İzleyici** ayrı bir sekmede veri tümleştirme uygulamayı başlatmak için döşeme.
   
   ![Data factory giriş sayfası](./media/load-azure-sql-data-warehouse/data-factory-home-page.png)

## <a name="load-data-into-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse'a veri yükleme

1. Get başlangıç sayfasını tıklatın **veri Kopyala** veri Kopyala aracını başlatmak için bölme. 

   ![Kopya veri aracı kutucuğu](./media/load-azure-sql-data-warehouse/copy-data-tool-tile.png)
2. İçinde **özellikleri** sayfasında veri Kopyala aracı belirtin **CopyFromSQLToSQLDW** için **görev adı**, tıklatıp **sonraki**. 

    ![Veri aracı-Özellikler sayfası kopyalama](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)
3. İçinde **kaynak veri deposu** sayfasında, **Azure SQL veritabanı**, tıklatıp **sonraki**.

    ![Kaynak veri deposu sayfası](./media/load-azure-sql-data-warehouse/specify-source.png)
4. İçinde **Azure SQL veritabanı belirleme** sayfasında, aşağıdaki adımları uygulayın: 
    1. Azure SQL sunucunuzu seçin **sunucu adı**.
    2. Azure SQL veritabanınızı seçin **veritabanı adı**.
    3. İçin kullanıcı adını belirtin **kullanıcı adı**.
    4. Kullanıcı için parola belirtin **parola**.
    5. **İleri**’ye tıklayın. 

        ![Azure SQL DB belirtin](./media/load-azure-sql-data-warehouse/specify-source-connection.png)
5. İçinde **veri kopyalama veya özel bir sorgu kullanmak üzere tabloları seçme** sayfasında, tablolar belirterek filtre **SalesLT** giriş kutusuna daha sonra denetleyin **(Tümünü Seç)** tüm tabloları seçin ve ardından onay kutusunu **sonraki**. 

    ![Girdi dosyası veya klasörü seçin](./media/load-azure-sql-data-warehouse/select-source-tables.png)

6. İçinde **hedef veri deposu** sayfasında, **Azure SQL Data Warehouse**, tıklatıp **sonraki**. 

    ![Hedef veri deposu sayfası](./media/load-azure-sql-data-warehouse/specify-sink.png)
7. İçinde **Azure SQL Data Warehouse belirtin** sayfasında, aşağıdaki adımları uygulayın: 

    1. Azure SQL sunucunuzu seçin **sunucu adı**.
    2. Azure SQL Data Warehouse için seçin **veritabanı adı**.
    3. İçin kullanıcı adını belirtin **kullanıcı adı**.
    4. Kullanıcı için parola belirtin **parola**.
    5. **İleri**’ye tıklayın. 

        ![Azure SQL veri ambarı belirtin](./media/load-azure-sql-data-warehouse/specify-sink-connection.png)
8. İçinde **tablo eşlemesi** sayfasında gözden geçirin ve tıklayın **sonraki**. Bir akıllı tablo eşlemesi eşlemeleri tablo adlarına göre hedef tabloları için kaynak görünür. Tablo içinde mevcut değilse hedef aynı ada sahip bir Azure Data Factory varsayılan olarak oluşturur. Var olan bir tabloya eşlemek seçebilirsiniz. 

    > [!NOTE]
    > SQL Server veya Azure SQL veritabanı kaynak olduğunda Otomatik Tablo oluşturma SQL Data Warehouse havuz için geçerlidir. Diğer kaynak veri deposundan verileri kopyalarsanız öncesi için şema havuz Azure SQL Data Warehouse veri kopyalama önce oluşturduğunuz.

    ![Tablo eşleme sayfası](./media/load-azure-sql-data-warehouse/specify-table-mapping.png)

9. İçinde **şema eşleme** sayfasında gözden geçirin ve tıklayın **sonraki**. Akıllı Eşleme sütun adını temel alır. Veri Fabrikası otomatik tabloları oluşturma olanak seçerseniz, uygun veri türü dönüşümü kaynak ve hedef depoları arasında uyumsuzluk sorunu düzeltmek için gerekirse ortaya çıkabilir. Kaynak ve hedef sütun arasında bir desteklenmeyen veri türü dönüştürme ise, ilgili tablo yanında bir hata iletisine bakın.

    ![Şema eşleme sayfası](./media/load-azure-sql-data-warehouse/specify-schema-mapping.png)

11. İçinde **ayarları** sayfasında, bir Azure depolama alanında seçin **depolama hesabı adı** aşağı açılan liste. PolyBase kullanarak SQL Data Warehouse içine yüklenmeden önce verileri hazırlamak için kullanılır. Kopyalama tamamlandıktan sonra depolama birimindeki geçici verileri otomatik olarak temizlenecek. 

    "Gelişmiş Ayarlar altında", "türü Varsayılanı kullan" seçeneğinin işaretini kaldırın.

    ![Ayarları sayfası](./media/load-azure-sql-data-warehouse/copy-settings.png)
12. İçinde **Özet** sayfasında, ayarları gözden geçirin ve tıklayın **sonraki**.

    ![Özet sayfası](./media/load-azure-sql-data-warehouse/summary-page.png)
13. İçinde **dağıtım sayfası**, tıklatın **İzleyici** (görev) işlem hattını izleme.

    ![Dağıtım sayfası](./media/load-azure-sql-data-warehouse/deployment-page.png)
14. Dikkat **İzleyici** sol sekmesinde otomatik olarak seçilir. Etkinlik ayrıntıları görüntülemek ve ardışık düzeninde yeniden çalıştırmak için bağlantılara bakın **Eylemler** sütun. 

    ![İşlem hattını izleme çalıştırır](./media/load-azure-sql-data-warehouse/monitor-pipeline-run.png)
15. Etkinlik çalışması çalıştırmak ardışık düzen ile ilişkili görüntülemek için **etkinlik görünümü çalışır** bağlamak **Eylemler** sütun. Ardışık düzeninde 10 kopyalama etkinlikleri vardır, her bir tablo veri kopyalar. Ardışık Düzen dönmeyi görünüm çalışır, tıklatın **ardışık düzen** üst bağlantı. Tıklatın **yenileme** listesini yenilemek için. 

    ![Monitör etkinliği çalıştırır](./media/load-azure-sql-data-warehouse/monitor-activity-run.png)

16. Daha fazla tıklayarak her kopya etkinliğin yürütme ayrıntıları izleyebilirsiniz **ayrıntıları** altında bağlantı **Eylemler** izleme görünümü etkinliğinde. Kaynak havuzu, işleme, ile ilgili süre geçtiği ve yapılandırmaları kullanılan adımları kopyalanan veri hacmi gibi bilgileri gösterir.

    ![Ayrıntılar Çalıştır etkinliğini izleme](./media/load-azure-sql-data-warehouse/monitor-activity-run-details.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure SQL Data Warehouse desteği hakkında bilgi edinmek için aşağıdaki makaleyi ilerletmek: 

> [!div class="nextstepaction"]
>[Azure SQL Data Warehouse Bağlayıcısı](connector-azure-sql-data-warehouse.md)