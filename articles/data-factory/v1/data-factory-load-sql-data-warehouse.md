---
title: "SQL Data Warehouse'a terabayt veri yükleme | Microsoft Docs"
description: "Azure Data Factory ile altında 15 dakika nasıl 1 TB'lık veriyi Azure SQL Data Warehouse'a yüklenebilir gösterir"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: a6c133c0-ced2-463c-86f0-a07b00c9e37f
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 3350645d4f173a6d0d007ff9095bb3115600a13b
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-data-factory"></a>1 TB altında 15 dakika Data Factory ile Azure SQL Data Warehouse'a veri yükleme
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [veri kopyalama ya da Azure SQL veri ambarından veri fabrikası sürüm 2 kullanarak](../connector-azure-sql-data-warehouse.md).


[Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) bir bulut tabanlı genişleme büyük miktarda veriyi, ilişkisel ve ilişkisel olmayan işleyebilen veritabanıdır.  SQL Data Warehouse, yüksek düzeyde paralel işleme (MPP) mimarisi üzerine kurulu, kurumsal veri ambarı iş yükleri için optimize edilmiştir.  Bu bulut esneklik depolama ölçeklendirmenizi ve bağımsız olarak işlem esnekliği sunar.

Azure SQL Data Warehouse ile çalışmaya başlama artık her zamankinden kullanmaktan daha kolay **Azure Data Factory**.  Azure Data Factory SQL Data Warehouse, var olan sisteminizden ve SQL Data Warehouse değerlendirmek ve analizlerinizi oluşturma, değerli zamandan kaydetme verilerle doldurmak için kullanılan bir bulut tabanlı tam olarak yönetilen bir veri tümleştirme hizmetidir çözümleri. Azure SQL Data Azure Data Factory kullanarak Warehouse'a veri yükleme avantajları şunlardır:

* **Ayarlamak kolay**: 5-adım sezgisel Sihirbazı ile gerekli komut dosyası yok.
* **Zengin veri depolamak Destek**: zengin bir şirket içi ve bulut tabanlı veri depoları için yerleşik destek.
* **Güvenli ve uyumlu**: veriler HTTPS veya ExpressRoute üzerinden aktarılır ve küresel hizmet varlığı sağlar, verilerinizin hiçbir zaman coğrafi sınır bırakır
* **PolyBase kullanarak benzersiz performansı** – Polybase kullanarak Azure SQL Data Warehouse'a veri taşımak için en verimli şekilde eder. Hazırlama blob özelliğini kullanarak, varsayılan olarak Polybase destekleyen Azure Blob Depolama yanı sıra veri depolarına tüm türlerinden yüksek yük hızları elde edebilirsiniz.

Bu makalede veri fabrikası Kopyalama Sihirbazı'nı tekrar 1,2 GB/sn üretilen işi en altında 15 dakika içinde 1 TB verileri Azure Blob depolama alanından Azure SQL Data Warehouse'a veri yüklemek için nasıl kullanılacağı gösterilmektedir.

Bu makalede Kopyalama Sihirbazı'nı kullanarak Azure SQL Data Warehouse'a veri taşımak için adım adım yönergeler sağlar.

> [!NOTE]
>  Veri taşımada/Azure SQL Data Warehouse genel veri fabrikasının özellikleri hakkında bilgi için bkz: [veri taşıma Azure Data Factory kullanarak Azure SQL veri ambarından](data-factory-azure-sql-data-warehouse-connector.md) makalesi.
>
> Ayrıca, Azure portalı, Visual Studio, PowerShell kullanılarak işlem hatlarını oluşturabilirsiniz vs. Bkz: [öğretici: kopyalama verileri Azure Blob'tan Azure SQL veritabanına](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hızlı bir kılavuz kopya etkinliği Azure Data Factory kullanarak için adım adım yönergeler için.  
>
>

## <a name="prerequisites"></a>Önkoşullar
* Azure Blob Storage: TPC-H sınama veri kümesi depolamak için bu denemeyi Azure Blob Depolama (GRS) kullanır.  Azure depolama hesabınız yoksa, bilgi [bir depolama hesabı oluşturmak nasıl](../../storage/common/storage-create-storage-account.md#create-a-storage-account).
* [TPC-H](http://www.tpc.org/tpch/) veri: biz TPC-H sınama veri kümesi kullanacağınız.  Bunu yapmak için kullanmanız gerekir `dbgen` TPC-H araç setinin yardımcı olan, veri kümesi oluşturur.  Kaynak kodu ya da indirebilirsiniz `dbgen` gelen [TPC Araçları](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) ve kendiniz derleme veya derlenmiş ikili dosyadan indirme [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).  Dbgen.exe için 1 TB düz dosya oluşturmak için aşağıdaki komutları çalıştırın `lineitem` yayılan 10 dosyalardaki tablosu:

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * …
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    Artık Azure Blob oluşturulan dosyaları kopyalayın.  Başvurmak [veri taşıma için ve bir şirket içi dosya sisteminden Azure Data Factory kullanarak](data-factory-onprem-file-system-connector.md) nasıl için ADF kopyalama kullanarak.    
* Azure SQL Data Warehouse: Azure SQL Data Warehouse 6,000 Dwu ile oluşturulan veri bu deneme yükler

    Başvurmak [bir Azure SQL Data Warehouse oluşturma](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) SQL Data Warehouse veritabanı oluşturma konusunda ayrıntılı yönergeler için.  En iyi olası yük performansı Polybase kullanarak SQL Data Warehouse'a veri almak için biz veri ambarı birimlerini (Dwu'lar) 6,000 Dwu olan performans ayarında, izin verilen en fazla sayısını seçin.

  > [!NOTE]
  > Azure Blob'tan yüklenirken veri performans yükleme SQL veri ambarını yapılandırma Dwu sayısı orantılıdır:
  >
  > 1 TB 1.000 yüklenirken DWU SQL Data Warehouse 87 dakika (~ 200 MB/sn verim) yükleme 1 TB 14 dakika (~1.2 GB/sn verim) alır 46 dakika (~ 380 MB/sn verim) yükleme 1 TB 6,000 DWU SQL veri ambarında sürer 2.000 DWU SQL Data Warehouse'a alır
  >
  >

    6000 Dwu ile SQL Data Warehouse oluşturmak için kaydırıcıyı sağa performans kaydırıcıyı taşıyın:

    ![Performans kaydırıcı](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    6000 Dwu ile yapılandırılmamış bir var olan veritabanı için bunu Azure portal kullanarak ölçeklendirebilirsiniz.  Azure portalında veritabanına gidin ve var. bir **ölçek** düğmesini **genel bakış** aşağıdaki görüntüde gösterilen paneli:

    ![Ölçek düğmesi](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Tıklatın **ölçek** aşağıdaki paneli, kaydırıcıyı en büyük değere ve'düğmesini **kaydetmek** düğmesi.

    ![Ölçek iletişim](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    Azure SQL Data Warehouse kullanarak verileri bu deneme yükler `xlargerc` kaynak sınıfı.

    Olası en iyi verime ulaşmak için kopyalama kullanarak ait bir SQL Data Warehouse kullanıcı gerçekleştirilmesi gerekir `xlargerc` kaynak sınıfı.  Aşağıdaki kullanarak bunu öğrenin [kullanıcı kaynak sınıfı örneğini değiştirmek](../../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md).  
* Hedef Tablo şemasını Azure SQL veri ambarı veritabanında aşağıdaki DDL deyimini çalıştırarak oluşturun:

    ```SQL  
    CREATE TABLE [dbo].[lineitem]
    (
        [L_ORDERKEY] [bigint] NOT NULL,
        [L_PARTKEY] [bigint] NOT NULL,
        [L_SUPPKEY] [bigint] NOT NULL,
        [L_LINENUMBER] [int] NOT NULL,
        [L_QUANTITY] [decimal](15, 2) NULL,
        [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
        [L_DISCOUNT] [decimal](15, 2) NULL,
        [L_TAX] [decimal](15, 2) NULL,
        [L_RETURNFLAG] [char](1) NULL,
        [L_LINESTATUS] [char](1) NULL,
        [L_SHIPDATE] [date] NULL,
        [L_COMMITDATE] [date] NULL,
        [L_RECEIPTDATE] [date] NULL,
        [L_SHIPINSTRUCT] [char](25) NULL,
        [L_SHIPMODE] [char](10) NULL,
        [L_COMMENT] [varchar](44) NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    ```
Önkoşul adımları tamamlanırken biz şimdi Kopyalama Sihirbazı'nı kullanarak kopyalama etkinliği yapılandırmaya hazırsınız demektir.

## <a name="launch-copy-wizard"></a>Kopyalama Sihirbazı'nı başlatma
1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Tıklatın **+ yeni** sol üst köşeden tıklatın **Intelligence + analiz**, tıklatıp **Data Factory**.
3. **Yeni data factory** dikey penceresinde:

   1. Girin **LoadIntoSQLDWDataFactory** için **adı**.
       Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Hatayı alırsanız: **veri fabrikası adı "LoadIntoSQLDWDataFactory" kullanılabilir değil**, veri fabrikası (örneğin, yournameLoadIntoSQLDWDataFactory) adını değiştirin ve oluşturmayı yeniden deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.  
   2. Azure **aboneliğinizi** seçin.
   3. Kaynak Grubu için aşağıdaki adımlardan birini uygulayın:
      1. Var olan bir kaynak grubu seçmek için **Var olanı kullan**’ı seçin.
      2. Bir kaynak grubunun adını girmek için **Yeni oluştur**’u seçin.
   4. Veri fabrikası için bir **konum** seçin.
   5. Dikey pencerenin alt kısmındaki **Panoya sabitle** onay kutusunu seçin.  
   6. **Oluştur**’a tıklayın.
4. Oluşturma işlemi tamamlandıktan sonra, aşağıdaki görüntüde gösterildiği gibi **Data Factory** dikey penceresini görürsünüz:

   ![Data factory giriş sayfası](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
5. Data Factory giriş sayfasında **Veri kopyala** kutucuğuna tıklayarak **Kopyalama Sihirbazı**’nı başlatın.

   > [!NOTE]
   > Web tarayıcısının "Yetkilendiriliyor..." durumunda takıldığını görürseniz **Üçüncü taraf tanımlama bilgilerini ve site verilerini engelle** ayarını devre dışı bırakın/işaretini kaldırın (ya da) etkin durumda bırakıp **login.microsoftonline.com** için bir özel durum oluşturun ve ardından sihirbazı yeniden başlatmayı deneyin.
   >
   >

## <a name="step-1-configure-data-loading-schedule"></a>1. adım: veri zamanlama yükleme yapılandırma
İlk adım, zamanlama yüklenirken veri yapılandırmaktır.  

**Özellikler** sayfasında:

1. Girin **CopyFromBlobToAzureSqlDataWarehouse** için **görev adı**
2. Seçin **kez Şimdi Çalıştır** seçeneği.   
3. **İleri**’ye tıklayın.  

    ![Kopyalama Sihirbazı - Özellikler sayfası](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>2. adım: kaynak yapılandırma
Bu bölümde, kaynak yapılandırma adımlarını gösterir: Azure 1 TB TPC içeren Blob-H satır öğesi dosyaları.

1. Seçin **Azure Blob Storage** olarak veri depolamak ve tıklatın **sonraki**.

    ![Kopyalama Sihirbazı - Kaynak Seç sayfası](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. Azure Blob Depolama hesabı bağlantı bilgileri girin ve tıklayın **sonraki**.

    ![Kopyalama Sihirbazı - kaynağı bağlantı bilgileri](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. Seçin **klasörü** öğesi dosyalarını satır ve tıklatın TPC-H içeren **sonraki**.

    ![Kopyalama Sihirbazı - giriş klasörü seçin](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. Tıklatarak bağlı **sonraki**, dosya biçimi ayarları otomatik olarak algılanır.  Bu sütun sınırlayıcı olduğundan emin olmak için onay ' | 'yerine varsayılan virgül','.  Tıklatın **sonraki** veri önizlemesi sonra.

    ![Kopyalama Sihirbazı - dosya biçimi ayarları](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>3. adım: hedef yapılandırma
Bu bölümde hedef yapılandırılacağı gösterilmiştir: `lineitem` Azure SQL Data Warehouse veritabanı tablosunda.

1. Seçin **Azure SQL Data Warehouse** hedef olarak depolamak ve tıklatın **sonraki**.

    ![Kopyalama Sihirbazı - select hedef veri deposu](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. Azure SQL Data Warehouse için bağlantı bilgilerini doldurun.  Rolünün bir üyesi olan kullanıcı belirttiğinizden emin olun `xlargerc` (bkz **Önkoşullar** ayrıntılı yönergeler için bölüm), tıklatıp **sonraki**.

    ![Kopyalama Sihirbazı - hedef bağlantı bilgileri](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. Hedef tablo seçin ve tıklatın **sonraki**.

    ![Kopyalama Sihirbazı - Tablo eşleme sayfası](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. Şema Eşleme sayfasında "Sütun eşlemesi uygulama" seçeneği işaretsiz bırakın ve tıklatın **sonraki**.

## <a name="step-4-performance-settings"></a>4. adım: Performans ayarları

**Polybase izin** varsayılan olarak işaretli.  **İleri**’ye tıklayın.

![Kopyalama Sihirbazı - şema eşleme sayfası](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>5. adım: Dağıtma ve yükleme sonuçları izleme
1. Tıklatın **son** dağıtmak için düğmesi.

    ![Kopyalama Sihirbazı - Özet sayfası](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. Dağıtım tamamlandıktan sonra tıklayın `Click here to monitor copy pipeline` Çalıştır ilerlemesi kopyalama izlemek için. Oluşturduğunuz kopyalama işlem hattını seçin **etkinlik Windows** listesi.

    ![Kopyalama Sihirbazı - Özet sayfası](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    Ayrıntılar Çalıştır kopyalama görüntüleyebilirsiniz **etkinlik penceresini Explorer** kaynaktan okunan ve hedef, süre ve ortalama verimi çalıştırma için yazılan veri hacmi sağ panelde dahil olmak üzere.

    Aşağıdaki ekran gördüğünüz gibi 1 TB Azure Blob depolama alanından SQL Data Warehouse'a veri kopyalama etkili bir şekilde 1.22 GBps verimlilik elde 14 dakika sürdü!

    ![Kopyalama Sihirbazı - başarılı oldu, iletişim kutusu](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>En iyi uygulamalar
Azure SQL Data Warehouse veritabanınızı çalıştırmak için birkaç en iyi uygulamalar şunlardır:

* Daha büyük bir kaynak sınıfı, bir kümelenmiş COLUMNSTORE DİZİNİNE yüklerken kullanın.
* Daha verimli birleştirmeler için hepsini bir kez deneme dağıtımı varsayılan yerine select bir sütuna göre karma dağıtım kullanmayı düşünün.
* İçin daha hızlı yük hızları, geçici verileri öbek kullanmayı düşünün.
* Azure SQL Data Warehouse yüklemeyi bitirdikten sonra İstatistikler oluşturun.

Bkz: [en iyi uygulamalar Azure SQL Data Warehouse için](../../sql-data-warehouse/sql-data-warehouse-best-practices.md) Ayrıntılar için.

## <a name="next-steps"></a>Sonraki adımlar
* [Data Factory Kopyalama Sihirbazı](data-factory-copy-wizard.md) -bu makalede Kopyalama Sihirbazı hakkında ayrıntılar sağlar.
* [Etkinlik performans ve ayarlama Kılavuzu kopyalama](data-factory-copy-activity-performance.md) -bu makalede başvuru performans ölçümleri ve ayarlama kılavuzu içerir.
