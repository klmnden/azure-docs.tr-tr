---
title: Azure SQL Data Warehouse'a – Data Factory veri yükleme | Microsoft Docs
description: Bu öğreticide Azure Data Factory kullanarak Azure SQL Data Warehouse'a veri yükler ve veri kaynağı olarak bir SQL Server veritabanını kullanır.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: ''
tags: azure-sql-data-warehouse;azure-data-factory
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: loading
ms.date: 02/08/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 6399f1a3390119685c1c9fd7332937e0cdb6f9ea
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2018
---
# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a>Data Factory ile SQL veri ambarına veri yükleme

Azure Data Factory herhangi birini Azure SQL Data Warehouse'a veri yüklemek için kullanabileceğiniz [desteklenen kaynak veri depoları](../data-factory/copy-activity-overview.md). Örneğin, verileri Azure SQL veritabanına veya bir Oracle veritabanından bir SQL data warehouse'a veri fabrikası kullanarak yükleyebilirsiniz. Bu makalede öğreticide bir şirket içi SQL Server veritabanından SQL data warehouse'a veri yükleme gösterilmektedir.

**Zaman tahmin**: Bu öğreticide Önkoşullar sağlandığında tamamlamak için yaklaşık 10-15 dakika sürer.

## <a name="prerequisites"></a>Önkoşullar

- Gereksinim duyduğunuz bir **SQL Server veritabanı** SQL veri ambarı'na üzerinden kopyalanacak verileri içeren tablolar ile.  

- Çevrimiçi ihtiyacınız **SQL Data Warehouse**. Veri ambarı zaten yoksa öğrenin nasıl [bir Azure SQL Data Warehouse oluşturma](sql-data-warehouse-get-started-provision.md).

- Gereksinim duyduğunuz bir **Azure depolama hesabı**. Bir depolama hesabı zaten yoksa öğrenin nasıl [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md). En iyi performans için depolama hesabı ve veri ambarı aynı Azure bölgesinde bulun.

## <a name="configure-a-data-factory"></a>Data factory Yapılandır
1. [Azure Portal][]’da oturum açın.
2. Veri ambarınız bulun ve açmak için tıklatın.
3. Ana dikey penceresinde tıklayın **veri yükleme** > **Azure Data Factory**.

    ![Veri Yükleme Sihirbazını Başlat](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)

4. Azure aboneliğinizde bir veri fabrikası yoksa gördüğünüz bir **yeni Data Factory** ayrı bir sekmesinde iletişim kutusunda tarayıcının. İstenen bilgileri girin ve tıklayın **oluşturma**. Veri Fabrikası oluşturulduktan sonra **yeni Data Factory** iletişim kutusunu kapatır ve gördüğünüz **seçin Data Factory** iletişim kutusu.

    Zaten Azure Abonelikteki bir veya daha fazla veri fabrikaları varsa, gördüğünüz **seçin Data Factory** iletişim kutusu. Bu iletişim kutusunda, mevcut bir veri fabrikasını seçin veya tıklatın **oluştur yeni data factory** yeni bir tane oluşturmak için.

    ![Veri Fabrikası yapılandırın](media/sql-data-warehouse-load-with-data-factory/configure-data-factory.png)

5. İçinde **seçin Data Factory** iletişim kutusu, **veri yükleme** seçeneği, varsayılan olarak seçilidir. Tıklatın **sonraki** veri görev yükleme oluşturmaya başlamak için.

## <a name="configure-the-data-factory-properties"></a>Veri Fabrikası özelliklerini yapılandırın
Veri Fabrikası oluşturduğunuza göre sonraki adıma Zamanlama yüklenirken veri yapılandırmaktır.

1. İçin **görev adı**, girin **DWLoadData fromSQLServer**.
2. Varsayılan kullanmak **kez Şimdi Çalıştır** seçeneği, tıklatın **sonraki**.

    ![Yükleme zamanlamasını yapılandırın](media/sql-data-warehouse-load-with-data-factory/configure-load-schedule.png)

## <a name="configure-the-source-data-store-and-gateway"></a>Ağ geçidi ve kaynak veri deposu yapılandırma
Şimdi veri yüklemek istediğiniz şirket içi SQL Server veritabanı hakkında veri fabrikası söyleyin.

1. Seçin **SQL Server** desteklenen kaynak verilerden katalog depolamak ve tıklatın **sonraki**.

    ![SQL Server kaynak seçin](media/sql-data-warehouse-load-with-data-factory/choose-sql-server-source.png)

2. A **şirket içi SQL Server veritabanını belirtin** iletişim kutusu görüntülenir. İlk **bağlantı adı** doldurulmuş otomatik bir alandır. İkinci alan adını ister **ağ geçidi**. Bir ağ geçidi zaten mevcut bir veri fabrikasını kullanıyorsanız, aşağı açılan listeden seçerek ağ geçidini yeniden kullanabilirsiniz. Tıklatın **ağ geçidi Oluştur** veri yönetimi ağ geçidi oluşturmak için bağlantı.  

    > [!NOTE]
    > Kaynak veri deposu, şirket içi veya bir Azure Iaas sanal makinesi veri yönetimi ağ geçidi gereklidir. Bir ağ geçidi, veri fabrikası bir 1-1 ilişkisi yok. Başka bir veri fabrikası'ndan kullanılamaz, ancak birden çok veri yükleme ile aynı veri fabrikasında görevleri tarafından kullanılabilir. Bir ağ geçidi, veri görevleri yükleme çalışırken birden çok veri depolarına bağlanmak için kullanılabilir.
    >
    > Ağ geçidi hakkında ayrıntılı bilgi için bkz: [veri yönetimi ağ geçidi](../data-factory/v1/data-factory-data-management-gateway.md) makalesi.

3. A **ağ geçidi Oluştur** iletişim kutusu görüntülenir. Adı **GatewayForDWLoading**, tıklatıp **oluşturma**.

4. A **yapılandırma ağ geçidi** iletişim kutusu görüntülenir. Tıklatın **başlatma hızlı kurulum bu bilgisayarda** otomatik olarak indirmek için yüklemek ve veri yönetimi ağ geçidi geçerli makinenize kaydedin. İlerleme açılır pencerede gösterilir. Makine veri deposuna bağlanamazsa, el ile denetleyebilirsiniz [ağ geçidi yükleyip](https://www.microsoft.com/download/details.aspx?id=39717) verilere bağlayabilirsiniz bir makinede depolamak ve kaydetmek için kayıt anahtarını kullanın.
    > [!NOTE]
    > Hızlı Kurulum, Microsoft Edge ve Internet Explorer ile yerel olarak çalışır. İlk Google Chrome kullanıyorsanız, ClickOnce uzantı Chrome web Mağazası'ndan yükleyin.

    ![Hızlı Kurulum başlatma](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

5. Ağ geçidi kurulumun tamamlanması için bekleyin. Ağ geçidi başarıyla kaydedildi ve çevrimiçi sonra açılır penceresi kapanır ve yeni ağ geçidi ağ geçidi alanında görüntülenir. Ardından kalan doldurun gibi gerekli alanları, ardından **sonraki**.
    - **Sunucu adı**: şirket içi SQL Server'ın adı.
    - **Veritabanı adı**: SQL Server veritabanı.
    - **Kimlik bilgisi şifreleme**: "web tarayıcısı tarafından" varsayılan kullanın.
    - **Kimlik doğrulama türü**: kullanmakta olduğunuz kimlik doğrulama türünü seçin.
    - **Kullanıcı adı** ve **parola**: veri kopyalamak için izni olan bir kullanıcı için kullanıcı adını ve parolasını girin.

    ![Hızlı Kurulum başlatma](media/sql-data-warehouse-load-with-data-factory/configure-sql-server.png)

6. Sonraki adım, verileri kopyalamak tablolardan seçmektir. Anahtar sözcükler kullanarak tabloları filtre uygulayabilirsiniz. Ve veri ve tablo şeması alt panelinde önizleyebilirsiniz. Seçiminiz tamamladıktan sonra tıklatın **sonraki**.

    ![Tabloları seçme](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-the-destination-your-sql-data-warehouse"></a>Hedef, SQL veri ambarı yapılandırma

Şimdi veri fabrikası hedef bilgilerini söyleyin.

1. SQL veri ambarı bağlantı bilgilerini otomatik olarak doldurulur. Kullanıcı adı için parola girin. tıklatıp **sonraki**.

    ![Hedef yapılandırma](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

2. Bir akıllı tablo eşlemesi eşlemeleri tablo adlarına göre hedef tabloları için kaynak görünür. Hedef Tablo mevcut değilse, varsayılan olarak (Bu bu SQL Server ya da kaynak olarak Azure SQL veritabanı için geçerlidir) aynı ada sahip bir ADF oluşturur. Var olan bir tabloya eşlemek seçebilirsiniz. Gözden geçirin ve tıklatın **sonraki**.

    ![Eşleme tabloları](media/sql-data-warehouse-load-with-data-factory/table-mapping.png)

3. Şema eşleme gözden geçirin ve hata veya uyarı iletilerini arayın. Akıllı Eşleme sütun adını temel alır. Kaynak ve hedef sütun arasında bir desteklenmeyen veri türü dönüştürme ise, ilgili tablo yanında bir hata iletisine bakın. Veri Fabrikası otomatik tabloları oluşturma olanak seçerseniz, uygun veri türü dönüşümü kaynak ve hedef depoları arasında uyumsuzluk sorunu düzeltmek için gerekirse ortaya çıkabilir.

    ![Şema eşleme](media/sql-data-warehouse-load-with-data-factory/schema-mapping.png)

4. **İleri**’ye tıklayın.

## <a name="configure-the-performance-settings"></a>Performans ayarlarını yapılandır
SQL veri ambarı performantly kullanarak içine yüklenmeden önce verileri hazırlamak için kullanılan bir Azure depolama hesabı yapılandırma performans yapılandırmalarında [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly). Kopyalama tamamlandıktan sonra depolama birimindeki geçici verileri otomatik olarak temizlenecek.

Mevcut bir Azure depolama hesabını seçin ve tıklayın **sonraki**.

![Hazırlama blob yapılandırın](media/sql-data-warehouse-load-with-data-factory/configure-staging-blob.png)

## <a name="review-summary-information-and-deploy-the-pipeline"></a>Özet bilgileri gözden geçirin ve ardışık düzen dağıtın

Yapılandırmasını gözden geçirmek ve tıklayın **son** düğmesi ardışık düzen dağıtın.

![Veri Fabrikası dağıtma](media/sql-data-warehouse-load-with-data-factory/deploy-data-factory.png)

## <a name="monitor-data-loading-progress"></a>Yükleme ilerlemesini izleme verileri

Sonuçları ve dağıtımının ilerleme durumunu görebilirsiniz **dağıtım** sayfası.

1. Dağıtım tamamlandığında bağlantısını tıklatın **kopyalama işlem hattını izlemek için burayı tıklatın** veri yükleme ilerlemesini izlemek için.

    ![İşlem hattını izleme](media/sql-data-warehouse-load-with-data-factory/monitor-pipeline.png)

2. Yeni oluşturulan **DWLoadData fromSQLServer** veri yükleme ardışık olan sol seçili otomatik **kaynak Gezgini**.

    ![Görünüm ardışık düzen](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

3. Ardışık Düzen Orta panelinde bir etkinliğe eşlemeleri her tablo için ayrıntılı durum görmek için tıklayın.

    ![Tablo etkinliği görüntüle](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

4. Daha fazla etkinliğin içinde tıklatın ve veri boyutu, satır, işleme, vb. dahil olmak üzere sağ bölmede ayrıntıları yükleniyor veri bakın.

    ![Tablo Etkinlik ayrıntıları görüntüle](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

5. Bu izleme görünümü daha sonra SQL veri ambarı gidin başlatmak için tıklatın **veri yükleme > Azure Data Factory**, fabrikanızı seçin ve **izleme görevleri yüklenirken varolan**.

## <a name="next-steps"></a>Sonraki adımlar

SQL Data Warehouse veritabanınızı geçirmek için bkz [geçişine genel bakış](sql-data-warehouse-overview-migrate.md).

Azure Data Factory ve veri taşıma özelliklerini hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Data Factory'ye giriş](../data-factory/introduction.md)
- [Kopyalama etkinliği kullanarak veri taşıma](../data-factory/copy-activity-overview.md)
- [Azure Data Factory kullanarak Azure'a/Azure'dan veri taşıma](../data-factory/connector-azure-sql-data-warehouse.md)

SQL Data Warehouse verilerinizi keşfetmek için aşağıdaki makalelere bakın:

- [Visual Studio ve SSDT ile SQL Data Warehouse'a bağlanma](sql-data-warehouse-query-visual-studio.md)
- [Görsel verilerinizi Power BI ile](sql-data-warehouse-get-started-visualize-with-power-bi.md).

<!-- Azure references -->
[Azure Portal]: https://portal.azure.com
