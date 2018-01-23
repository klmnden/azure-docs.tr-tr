---
title: "Azure Veri Kopyalama aracını kullanarak veri kopyalama | Microsoft Docs"
description: "Azure blob depolama alanındaki bir klasörde bulunan verileri başka bir klasöre kopyalamak için bir Azure veri fabrikası oluşturun ve Veri Kopyalama aracını kullanın."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/16/2018
ms.author: jingwang
ms.openlocfilehash: c17e738e3db18503f7cdda24a5f01ceb78e4e6a3
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="use-copy-data-tool-to-copy-data"></a>Veri Kopyalama aracını kullanarak veri kopyalama 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Sürüm 2 - Önizleme](quickstart-create-data-factory-copy-data-tool.md)

Bu hızlı başlangıçta Azure portalını kullanarak bir veri fabrikası oluşturursunuz. Sonra, Veri Kopyalama aracını kullanarak Azure blob depolama alanındaki bir klasörde bulunan verileri başka bir klasöre kopyalarsınız. 

> [!NOTE]
> Azure Data Factory kullanmaya yeni başlıyorsanız, bu hızlı başlangıçtaki işlemleri gerçekleştirmeden önce [Azure Data Factory'ye giriş](data-factory-introduction.md) konusuna bakın. 
>
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 ile çalışmaya başlama](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.


[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **ad** için **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası sayfası](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory) ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
     ![Ad yok - hata](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
      Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı, vb.) ve işlemler (HDInsight, vb.) başka konumlarda/bölgelerde olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/quickstart-create-data-factory-copy-data-tool/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, görüntüde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
   ![Data factory giriş sayfası](./media/quickstart-create-data-factory-copy-data-tool/data-factory-home-page.png)
10. Azure Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Geliştir ve İzle** kutucuğuna tıklayın. 

## <a name="launch-copy-data-tool"></a>Veri Kopyalama aracını başlatma

1. Veri Kopyalama aracını başlatmak için başlangıç sayfasında **Veri Kopyalama**’ya tıklayın. 

   ![Veri Kopyalama aracının kutucuğu](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-tile.png)
2. Veri Kopyalama aracının **Özellikler** sayfasında **İleri**’ye tıklayın. Bu sayfada işlem hattının adını ve açıklamasını belirtebilirsiniz. 

    ![Veri Kopyalama aracı - Özellikler sayfası](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-properties-page.png)
3. **Kaynak veri deposu** sayfasında **Azure Blob Depolama**’yı seçip **İleri**’ye tıklayın.

    ![Kaynak veri deposu sayfası](./media/quickstart-create-data-factory-copy-data-tool/source-data-store-page.png)
4. **Azure Blob depolama hesabı belirtin** sayfasında, açılan listeden **depolama hesabınızın adını** seçip İleri’ye tıklayın. 

    ![Blob depolama hesabını belirtme](./media/quickstart-create-data-factory-copy-data-tool/specify-blob-storage-account.png)
5. **Girdi dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:

    1. **adftutorial/input** klasörüne gidin. 
    2. **emp.txt** dosyasını seçin.
    3. **Seçin**’e tıklayın. Bu adımı atlamak için **emp.txt** dosyasına çift tıklayabilirsiniz. 
    4. **İleri**’ye tıklayın. 

    ![Girdi dosyasını veya klasörünü seçin](./media/quickstart-create-data-factory-copy-data-tool/choose-input-file-folder.png)
6. **Dosya biçimi ayarları** sayfasında aracın sütun ve satır sınırlayıcılarını otomatik olarak belirlediğine dikkat edin ve **İleri**’ye tıklayın. Ayrıca, bu sayfada verilerin önizlemesini yapabilir ve giriş verilerinin şemasını görüntüleyebilirsiniz. 

    ![Dosya biçimi ayarları sayfası](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-page.png)
7. **Hedef veri deposu** sayfasında **Azure Blob Depolama**’yı seçip **İleri**’ye tıklayın. 

    ![Hedef veri deposu sayfası](./media/quickstart-create-data-factory-copy-data-tool/destination-data-store-page.png)    
8. **Azure Blob depolama hesabı belirtin** sayfasında, Azure Blob depolama hesabınızın adını seçip **İleri**’ye tıklayın. 

    ![Havuz veri deposu sayfasını belirtme](./media/quickstart-create-data-factory-copy-data-tool/specify-sink-blob-storage-account.png)
9. **Çıktı dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın: 

    1. **Klasör yolu** için **adftutorial/output** yolunu girin.
    2. **Dosya adı** için **emp.txt** adını girin. 
    3. **İleri**’ye tıklayın. 

    ![Çıktı dosyasını veya klasörünü seçin](./media/quickstart-create-data-factory-copy-data-tool/choose-output-file-folder.png) 
10. **Dosya biçimi ayarları** sayfasında **İleri**’ye tıklayın. 

    ![Dosya biçimi ayarları sayfası](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-output-page.png)
11. **Ayarlar** sayfasında **İleri**’ye tıklayın. 

    ![Gelişmiş ayarlar sayfası](./media/quickstart-create-data-factory-copy-data-tool/advanced-settings-page.png)
12. **Özet** sayfasında tüm ayarları gözden geçirin ve İleri’ye tıklayın. 

    ![Özet sayfası](./media/quickstart-create-data-factory-copy-data-tool/summary-page.png)
13. **Dağıtım tamamlandı** sayfasında, oluşturduğunuz işlem hattını izlemek için **İzleyici**’ye tıklayın. 

    ![Dağıtım sayfası](./media/quickstart-create-data-factory-copy-data-tool/deployment-page.png)
14. Uygulama **İzleyici** sekmesine geçer. Bu sayfada işlem hattının durumunu görürsünüz. Listeyi yenilemek için **Yenile**’ye tıklayın. 
    
    ![İşlem hattı çalıştırmalarını izleme sayfası](./media/quickstart-create-data-factory-copy-data-tool/monitor-pipeline-runs-page.png)
15. Eylemler sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısına tıklayın. İşlem hattı, **Kopyalama** türünde tek bir etkinlik içerir. 

    ![Etkinlik Çalıştırmaları sayfası](./media/quickstart-create-data-factory-copy-data-tool/activity-runs.png)
16. Kopyalama işlemiyle ilgili ayrıntıları görüntülemek için **Eylemler** sütunundaki **Ayrıntılar**’a (gözlük resmi) tıklayın. Özelliklerle ilgili ayrıntılar için bkz. [Kopyalama Etkinliğine genel bakış](copy-activity-overview.md). 

    ![İşlem ayrıntılarını kopyalama](./media/quickstart-create-data-factory-copy-data-tool/copy-operation-details.png)
17. **adftutorial** kapsayıcısının **çıktı** klasöründe **emp.txt** dosyasının oluşturulduğunu doğrulayın. Çıktı klasörü mevcut değilse Data Factory hizmeti tarafından otomatik olarak oluşturulur. 
18. Bağlı hizmetleri, veri kümelerini ve işlem hatlarını düzenleyebilmek için **Düzenle** sekmesine geçin. Bunları Data Factory kullanıcı arabiriminde düzenleme hakkında bilgi edinmek için bkz. [Azure portalını kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-portal.md).

    ![Düzenle sekmesi](./media/quickstart-create-data-factory-copy-data-tool/edit-tab.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure blob depolama alanındaki başka bir konuma kopyalar. Daha fazla senaryoda Data Factory’yi kullanma hakkında bilgi almak için [öğreticileri](tutorial-copy-data-portal.md) okuyun. 