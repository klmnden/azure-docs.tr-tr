---
title: Azure Veri Kopyalama aracını kullanarak veri kopyalama | Microsoft Docs
description: Azure Blob depolama alanındaki bir konumda bulunan verileri başka bir konuma kopyalamak için bir Azure veri fabrikası oluşturun ve Veri Kopyalama aracını kullanın.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/16/2018
ms.author: jingwang
ms.openlocfilehash: d09422f31a2dda5e14fb891fa07f65fdcceb72c1
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="use-the-copy-data-tool-to-copy-data"></a>Veri Kopyalama aracını kullanarak veri kopyalama 
> [!div class="op_single_selector" title1="Select the version of Data Factory service that you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Sürüm 2 - Önizleme](quickstart-create-data-factory-copy-data-tool.md)

Bu hızlı başlangıçta Azure portalını kullanarak bir veri fabrikası oluşturursunuz. Sonra, Veri Kopyalama aracını kullanarak Azure Blob depolama alanındaki bir klasörde bulunan verileri başka bir klasöre kopyalarsınız. 

> [!NOTE]
> Azure Data Factory'yi kullanmaya yeni başlıyorsanız, bu hızlı başlangıçtaki işlemleri gerçekleştirmeden önce [Azure Data Factory'ye giriş](data-factory-introduction.md) konusuna bakın. 
>
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Hizmetin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 ile çalışmaya başlama](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.


[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüden **Yeni**’yi, sonra **Veri ve Analiz**’i ve ardından **Data Factory**’i seçin. 
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **Ad** için **ADFTutorialDataFactory** girin. 
      
   ![“Yeni veri fabrikası” sayfası](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory.png)
 
   Azure data factory adı *küresel olarak benzersiz* olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin **&lt; adınız&gt;ADFTutorialDataFactory**) ve veri fabrikası oluşturmayı yeniden deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - adlandırma kuralları](naming-rules.md) makalesini inceleyin.
  
   ![Bir ad kullanılamadığında alınan hata](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. **Abonelik** için, veri fabrikasını oluşturmak istediğiniz Azure aboneliğini seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
   - **Var olanı kullan**’ı ve ardından listeden var olan bir kaynak grubunu seçin. 
   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
   Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. **Konum** için, veri fabrikasının konumunu seçin. 

   Bu listede yalnızca desteklenen konumlar görüntülenir. Data Factory tarafından kullanılan veri depoları (Azure Depolama ve Azure SQL Veritabanı gibi) ve işlemler (Azure HDInsight gibi) başka konumlarda veya bölgelerde olabilir.

6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’u seçin.
8. Panoda, **Data Factory Dağıtılıyor** durumuna sahip aşağıdaki kutucuğu görürsünüz: 

    ![“Data Factory Dağıtılıyor” kutucuğu](media/quickstart-create-data-factory-copy-data-tool/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, **Data Factory** sayfasını görürsünüz. Azure Data Factory kullanıcı arabirimi (UI) uygulamasını ayrı bir sekmede başlatmak için **Yazar ve İzleyici** kutucuğunu seçin.
   
   ![Veri fabrikasının “Yazar ve İzleyici” kutucuğuna sahip ana sayfası](./media/quickstart-create-data-factory-copy-data-tool/data-factory-home-page.png)

## <a name="start-the-copy-data-tool"></a>Veri Kopyalama aracını başlatma

1. **Başlayalım** sayfasında, Veri Kopyalama aracını başlatmak için **Veri Kopyala** kutucuğunu seçin. 

   ![“Veri kopyala” kutucuğu](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-tile.png)
2. Veri Kopyalama aracının **Özellikler** sayfasında **İleri**’yi seçin. Bu sayfada işlem hattının adını ve açıklamasını belirtebilirsiniz. 

   ![“Özellikler” sayfası](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-properties-page.png)
3. **Kaynak veri deposu** sayfasında **Azure Blob Depolama**’yı ve ardından **İleri**’yi seçin.

   ![“Kaynak veri deposu” sayfası](./media/quickstart-create-data-factory-copy-data-tool/source-data-store-page.png)
4. **Azure Blob depolama hesabı belirtin** sayfasında, **Depolama hesabı adı** listesinden depolama hesabınızı ve sonra **İleri**’yi seçin. 

   ![“Azure Blob depolama hesabını belirtin” sayfası](./media/quickstart-create-data-factory-copy-data-tool/specify-blob-storage-account.png)
5. **Girdi dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:

   a. **adftutorial/input** klasörüne göz atın.

   b. **emp.txt** dosyasını seçin.

   c. **Seç** seçeneğini belirleyin. Bu adımı atlamak için **emp.txt** dosyasına çift tıklayabilirsiniz.

   d. **İleri**’yi seçin. 

   ![“Girdi dosyası veya klasörü seçin” sayfası](./media/quickstart-create-data-factory-copy-data-tool/choose-input-file-folder.png)
6. **Dosya biçimi ayarları** sayfasında aracın sütun ve satır sınırlayıcılarını otomatik olarak belirlediğine dikkat edin ve **İleri**’yi seçin. Ayrıca, bu sayfada verilerin önizlemesini yapabilir ve giriş verisi şemalarını görüntüleyebilirsiniz. 

   ![“Dosya biçimi ayarları” sayfası](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-page.png)
7. **Hedef veri deposu** sayfasında **Azure Blob Depolama**’yı ve sonra **İleri**’yi seçin. 

   ![“Hedef veri deposu” sayfası](./media/quickstart-create-data-factory-copy-data-tool/destination-data-store-page.png)    
8. **Azure Blob depolama hesabı belirtin** sayfasında, Azure Blob depolama hesabınızın adını ve sonra **İleri**’yi seçin. 

   ![“Azure Blob depolama hesabını belirtin” sayfası](./media/quickstart-create-data-factory-copy-data-tool/specify-sink-blob-storage-account.png)
9. **Çıktı dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın: 

   a. Klasör yolu için **adftutorial/output** yolunu girin.

   b. Dosya adı için **emp.txt** adını girin.

   c. **İleri**’yi seçin. 

   ![“Çıktı dosyasını veya klasörünü seçin” sayfası](./media/quickstart-create-data-factory-copy-data-tool/choose-output-file-folder.png) 
10. **Dosya biçimi ayarları** sayfasında **İleri**’yi seçin. 

    ![“Dosya biçimi ayarları” sayfası](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-output-page.png)
11. **Ayarlar** sayfasında **İleri**’yi seçin. 

    ![“Ayarlar” sayfası](./media/quickstart-create-data-factory-copy-data-tool/advanced-settings-page.png)
12. **Özet** sayfasında tüm ayarları gözden geçirin ve **İleri**’yi seçin. 

    ![“Özet” sayfası](./media/quickstart-create-data-factory-copy-data-tool/summary-page.png)
13. **Dağıtım tamamlandı** sayfasında, oluşturduğunuz işlem hattını izlemek için **İzleyici**’yi seçin. 

    ![“Dağıtım tamamlandı” sayfası](./media/quickstart-create-data-factory-copy-data-tool/deployment-page.png)
14. Uygulama **İzleyici** sekmesine geçer. Bu sekmede işlem hattının durumunu görürsünüz. Listeyi yenilemek için **Yenile**’yi seçin. 
    
    ![“Yenile” düğmeli işlem hattı çalıştırmalarını izleme sekmesi](./media/quickstart-create-data-factory-copy-data-tool/monitor-pipeline-runs-page.png)
15. **Eylemler** sütununda **Etkinlik Çalıştırma İşlemlerini Görüntüle**’yi seçin. İşlem hattı, **Kopyalama** türünde tek bir etkinlik içerir. 

    ![Etkinlik çalıştırma işlemlerini görüntüleme](./media/quickstart-create-data-factory-copy-data-tool/activity-runs.png)
16. Kopyalama işlemiyle ilgili ayrıntıları görüntülemek için **Eylemler** sütunundaki **Ayrıntılar**’ı (gözlük resmi) seçin. Özelliklerle ilgili ayrıntılar için bkz. [Kopyalama Etkinliğine genel bakış](copy-activity-overview.md). 

    ![İşlem ayrıntılarını kopyalama](./media/quickstart-create-data-factory-copy-data-tool/copy-operation-details.png)
17. **adftutorial** kapsayıcısının **çıktı** klasöründe **emp.txt** dosyasının oluşturulduğunu doğrulayın. Çıkış klasörü yoksa, Data Factory hizmeti tarafından otomatik olarak oluşturulur. 
18. Bağlı hizmetleri, veri kümelerini ve işlem hatlarını düzenleyebilmek için **Düzenle** sekmesine geçin. Bunları Data Factory kullanıcı arabiriminde düzenleme hakkında bilgi edinmek için bkz. [Azure portalını kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-portal.md).

    ![Düzenle sekmesi](./media/quickstart-create-data-factory-copy-data-tool/edit-tab.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure Blob depolama alanındaki başka bir konuma kopyalar. Data Factory’yi daha fazla senaryoda kullanma hakkında bilgi almak için [öğreticileri](tutorial-copy-data-portal.md) okuyun. 