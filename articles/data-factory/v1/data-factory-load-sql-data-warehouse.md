---
title: SQL veri ambarı'na terabaytlarca veri yükleme | Microsoft Docs
description: Azure Data Factory ile 15 dakika nasıl 1 TB veri Azure SQL veri ambarı'na yüklenebilir gösterir
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: a6c133c0-ced2-463c-86f0-a07b00c9e37f
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: e275411f9fd9dfb672bb0815e83e37bcd5d1dda9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60825447"
---
# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-data-factory"></a>1 TB 15 dakika Data Factory ile Azure SQL Data Warehouse'a veri yükleme
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [veri kopyalama için veya Azure SQL veri ambarı Data Factory kullanarak](../connector-azure-sql-data-warehouse.md).


[Azure SQL veri ambarı](../../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) bir bulut tabanlı genişleme veriler, ilişkisel ve ilişkisel olmayan çok geniş hacimlerdeki işleyebilen veritabanıdır.  SQL veri ambarı, yüksek düzeyde paralel işleme (MPP) mimaride oluşturulmuş kurumsal veri ambarı iş yükleri için optimize edilmiştir.  Bulut esnekliği bağımsız olarak işlem ve depolama ölçeklendirme yapma esnekliği sunar.

Azure SQL veri ambarı ile çalışmaya başlama, artık her zamankinden kullanmaktan daha kolay **Azure Data Factory**.  Azure Data Factory, SQL veri ambarı mevcut sistemi ve SQL veri ambarı değerlendirmek ve analizlerinizi oluşturmaya çalışırken, önemli ölçüde zaman kaydetme verilerle doldurmak için kullanılan bir tam olarak yönetilen bulut tabanlı veri tümleştirme hizmeti çözümler. Azure SQL Data Azure Data Factory kullanarak Warehouse'a veri yükleme önemli avantajları şunlardır:

* **Kolay ayarlama**: hiçbir gerekli komut ile sezgisel Sihirbazı 5 adımlı.
* **Zengin veri deposu desteği**: zengin bir şirket içi ve bulut tabanlı veri depoları için yerleşik destek.
* **Güvenli ve uyumlu**: verileri HTTPS veya ExpressRoute üzerinden aktarılır ve küresel hizmet varlığı sağlar verilerinizin hiçbir zaman ayrılmaz coğrafi sınır
* **PolyBase kullanarak benzersiz bir performansın** – Polybase kullanarak, Azure SQL Data Warehouse'a veri taşımak için en verimli yoludur. Hazırlama blob özelliğini kullanarak Azure Blob Depolama, Polybase varsayılan destekliyor yanı sıra veri depolarının tüm türlerinden yüksek yükleme hızları elde edebilirsiniz.

Bu makalede Data Factory Kopyalama Sihirbazı'nı tekrar 1,2 GB/sn aktarım hızı anda altında 15 dakika içinde 1 TB verileri Azure Blob depolama alanından Azure SQL veri ambarı'na yüklemek için nasıl kullanılacağı gösterilmektedir.

Bu makalede, kopyalama Sihirbazı'nı kullanarak Azure SQL Data Warehouse'a veri taşımak için adım adım yönergeler sağlar.

> [!NOTE]
>  / Azure SQL veri ambarı veri taşımada genel Data Factory özellikleri hakkında daha fazla bilgi için [gelen Azure Data Factory kullanarak Azure SQL veri ambarı ve veri taşıma](data-factory-azure-sql-data-warehouse-connector.md) makalesi.
>
> Ayrıca, Azure portalı, Visual Studio, PowerShell kullanarak işlem hatları oluşturabilirsiniz vs. Bkz: [Öğreticisi: Verileri Azure Blobundan Azure SQL veritabanı'na kopyalamak](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliğini kullanarak Azure Data Factory'de için adım adım yönergeleri içeren hızlı bir kılavuz.  
>
>

## <a name="prerequisites"></a>Önkoşullar
* Azure Blob Depolama: Bu deneyde Azure Blob Depolama (GRS) TPC-H sınama veri kümesi depolamak için kullanır.  Azure depolama hesabınız yoksa, bilgi [bir depolama hesabının nasıl oluşturulacağını](../../storage/common/storage-quickstart-create-account.md).
* [TPC-H](http://www.tpc.org/tpch/) veri: TPC-H sınama veri kümesi olarak kullanmak için kullanacağız.  Bunu yapmak için kullanmanız gerekir `dbgen` TPC-H araç setinin yardımcı olan, veri kümesi oluştur.  Ya da kaynak kodunu indirebilirsiniz `dbgen` gelen [TPC Araçları](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) , kendiniz derleyebilir veya derlenmiş ikili dosyadan yükle [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).  Dbgen.exe için 1 TB düz dosya oluşturmak için aşağıdaki komutları çalıştırın `lineitem` yayılma 10 dosyalardaki Tablo:

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * …
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    Artık Azure Blob için oluşturulan dosyaları kopyalayın.  Başvurmak [veri taşıma, şirket içi dosya sisteminden bir Azure Data Factory kullanarak](data-factory-onprem-file-system-connector.md) bunu nasıl yapacağınız için ADF kopyalama kullanarak.    
* Azure SQL veri ambarı: Bu deneme 6000 Dwu ile oluşturulan Azure SQL veri ambarı'na veri yükler

    Başvurmak [bir Azure SQL Data Warehouse oluşturma](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) SQL Data Warehouse veritabanı oluşturma hakkında ayrıntılı yönergeler için.  En iyi olası bir yük performansı Polybase kullanarak SQL Data Warehouse'a veri almak için biz veri ambarı birimi (Dwu) 6000 dwu'ları olan performans ayarında, izin verilen en fazla sayısını seçin.

  > [!NOTE]
  > Azure Blobundan yüklenirken, yükleme performansını verileri SQL veri ambarı yapılandırma Dwu sayısı orantılıdır:
  >
  > 1\. 000'içinde 1 TB yükleme DWU SQL veri ambarı 87 dakika (yaklaşık 200 MB/sn Aktarım Hızı) yüklenirken 1 TB alır 46 dakika (~ 380 MB/sn Aktarım Hızı) yüklenirken 1 TB 6000 DWU SQL veri ambarı'na 14 dakika (~1.2 GB/sn Aktarım Hızı) sürer 2.000 DWU SQL Data Warehouse'a alır
  >
  >

    6000 Dwu ile SQL veri ambarı oluşturmak için sağındaki tüm performans kaydırıcıyı taşıyın:

    ![Performans kaydırıcı](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    6000 Dwu ile yapılandırılmamış var olan bir veritabanı için Azure portalını kullanarak ölçeklendiremezsiniz.  Azure portalında veritabanı gidin ve var olan bir **ölçek** düğmesine **genel bakış** paneli aşağıdaki görüntüde gösterilen:

    ![Ölçek düğmesi](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Tıklayın **ölçek** aşağıdaki paneli, kaydırıcıyı maksimum değerine ve'düğmesini **Kaydet** düğmesi.

    ![Ölçek iletişim](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    Bu denemeyi Azure SQL veri ambarı kullanarak verileri yükler `xlargerc` kaynak sınıfı.

    Mümkün olan en iyi performans sağlamak için kopyalama kullanarak ait bir SQL veri ambarı kullanıcı gerçekleştirilmesi gerekiyor `xlargerc` kaynak sınıfı.  Bunu izleyerek öğrenin [kullanıcı kaynak sınıfı örneğini değiştirmek](../../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md).  
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
  Önkoşul adımları tamamlanmış artık Kopyalama Sihirbazı'nı kullanarak kopyalama etkinliği yapılandırmak hazırız.

## <a name="launch-copy-wizard"></a>Kopyalama Sihirbazı'nı başlatma
1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Tıklayın **kaynak Oluştur** sol üst köşesdeki **zeka + analiz**, tıklatıp **Data Factory**.
3. İçinde **yeni veri fabrikası** bölmesi:

   1. Girin **LoadIntoSQLDWDataFactory** için **adı**.
       Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Hatayı alırsanız: **Veri Fabrikası adı "LoadIntoSQLDWDataFactory" kullanılamıyor**(örneğin, yournameLoadIntoSQLDWDataFactory) veri fabrikasının adını değiştirin ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.  
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

## <a name="step-1-configure-data-loading-schedule"></a>1\. adım: Zamanlama yükleniyor verilerini Yapılandır
İlk adım, veri yükleme zamanlaması yapılandırmaktır.  

**Özellikler** sayfasında:

1. Girin **CopyFromBlobToAzureSqlDataWarehouse** için **görev adı**
2. Seçin **şimdi bir kez çalıştır** seçeneği.   
3. **İleri**’ye tıklayın.  

    ![Kopyalama Sihirbazı - Özellikler sayfası](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>2\. adım: Kaynağı yapılandırma
Bu bölümde, kaynak yapılandırma adımları gösterilir: Azure Blob içeren 1 TB TPC-H satır öğesi dosyaları.

1. Seçin **Azure Blob Depolama** verileri depolamak ve tıklayın **sonraki**.

    ![Kopyalama Sihirbazı'nı - Kaynak Seç sayfası](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. Azure Blob Depolama hesabı için bağlantı bilgilerini girin ve tıklatın **sonraki**.

    ![Kopyalama Sihirbazı'nı - kaynak bağlantı bilgisi](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. Seçin **klasör** TPC-H içeren satır öğesi dosyaları ve tıklayın **sonraki**.

    ![Kopyalama Sihirbazı - giriş klasörü seçin](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. Üzerine tıklayarak **sonraki**, dosya biçimi ayarları otomatik olarak algılanır.  Bu sütun sınırlayıcısı olduğundan emin olmak için onay ' | 'yerine varsayılan virgül','.  Tıklayın **sonraki** sonra verilerin önizlemesini görebilirsiniz.

    ![Kopyalama Sihirbazı'nı - dosya biçimi ayarları](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>3\. adım: Hedef yapılandırma
Bu bölümde, hedef yapılandırma işlemini göstermektedir: `lineitem` Azure SQL veri ambarı veritabanındaki tablo.

1. Seçin **Azure SQL veri ambarı** hedef olarak depolamak ve tıklayın **sonraki**.

    ![-Select hedef veri deposuna Kopyalama Sihirbazı'nı](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. Azure SQL veri ambarı için bağlantı bilgilerini girin.  Kullanıcı rolünün bir üyesi belirttiğinizden emin olun `xlargerc` (bkz **önkoşulları** bölümünde ayrıntılı yönergeler için), tıklatıp **sonraki**.

    ![Kopyalama Sihirbazı'nı - hedef bağlantı bilgisi](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. Hedef tablo seçin ve tıklayın **sonraki**.

    ![Kopyalama Sihirbazı - Tablo eşleme sayfası](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. Şema eşleme sayfası içinde "Sütun eşlemesi uygulama" seçeneği işaretlemeden bırakıp tıklayın **sonraki**.

## <a name="step-4-performance-settings"></a>4\. Adım: Performans ayarları

**Polybase izin** varsayılan olarak işaretlidir.  **İleri**’ye tıklayın.

![Kopyalama Sihirbazı - şema eşleme sayfası](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>5\. Adım: Dağıtma ve yükleme sonuçları izleme
1. Tıklayın **son** dağıtmak için düğme.

    ![Kopyalama Sihirbazı - Özet sayfası](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. Dağıtım tamamlandıktan sonra tıklayın `Click here to monitor copy pipeline` ilerleme çalıştırma kopyalama izlemek için. Oluşturduğunuz kopyalama işlem hattını seçin **etkinlik Windows** listesi.

    ![Kopyalama Sihirbazı - Özet sayfası](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    Çalıştırma ayrıntıları Kopyala görüntüleyebileceğiniz **etkinlik penceresi Gezgini** kaynağından okumak ve hedef, süre ve ortalama aktarım hızı çalıştırma için yazılan veri hacmi sağ bölmede, dahil.

    Aşağıdaki ekran görüntüsünden görebileceğiniz gibi 1 TB Azure Blob depolama alanından SQL Data Warehouse'a veri kopyalama etkili bir şekilde 1.22 GB/sn aktarım hızı elde 14 dakika sürdü!

    ![Kopyalama Sihirbazı - başarılı iletişim kutusu](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>En iyi uygulamalar
Azure SQL Data Warehouse veritabanınıza çalıştırmaya yönelik bazı en iyi uygulamalar şunlardır:

* Kümelenmiş COLUMNSTORE bir DİZİNE yüklemeye daha büyük bir kaynak sınıfı kullanın.
* Daha verimli birleşimler için varsayılan hepsini bir kez deneme dağıtım yerine bir seçim sütunu karma dağıtım kullanarak göz önünde bulundurun.
* Daha hızlı yükleme hızları için geçici veriler için yığın kullanmayı düşünün.
* Azure SQL veri ambarı yükleme işlemini tamamladıktan sonra istatistik oluşturma.

Bkz: [en iyi uygulamalar için Azure SQL veri ambarı](../../sql-data-warehouse/sql-data-warehouse-best-practices.md) Ayrıntılar için.

## <a name="next-steps"></a>Sonraki adımlar
* [Data Factory Kopyalama Sihirbazı](data-factory-copy-wizard.md) -bu makalede Kopyalama Sihirbazı'nı hakkında ayrıntılar sağlar.
* [Etkinlik performansı ve ayarlama Kılavuzu kopyalama](data-factory-copy-activity-performance.md) -bu makalede başvuru performans ölçümleri ve ayar kılavuzu içerir.
