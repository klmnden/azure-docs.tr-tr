---
title: Artımlı olarak yeni dosyaları kopyalamak için Azure Data Factory kullanarak yalnızca zaman bölümlenmiş dosya adına göre | Microsoft Docs
description: Bir Azure veri fabrikası oluşturun ve ardından yeni dosyaların yalnızca zaman bölümlenmiş dosya adına göre artımlı olarak yükleme için veri kopyalama aracını kullanın.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 1/24/2019
ms.openlocfilehash: c89764d746f07e6100b1f250d4c107bb700fe014
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445844"
---
# <a name="incrementally-copy-new-files-based-on-time-partitioned-file-name-by-using-the-copy-data-tool"></a>Artımlı olarak veri kopyalama aracını kullanarak zaman bölümlenmiş dosya adına göre yeni dosyaları kopyalama

Bu öğreticide, Azure portalını kullanarak bir veri fabrikası oluşturursunuz. Ardından, yeni dosyaları Azure Blob depolamadan Azure Blob Depolama, zaman bölümlenmiş dosya adına göre artımlı olarak kopyalayan bir işlem hattı oluşturmak için veri kopyalama aracını kullanın. 

> [!NOTE]
> Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak bir işlem hattı oluşturun.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Önkoşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**: BLOB Depolama alanı olarak kullanabilmeniz _kaynak_ ve _havuz_ veri deposu. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md) bölümündeki yönergelere bakın.

### <a name="create-two-containers-in-blob-storage"></a>Blob Depolama alanında iki kapsayıcı oluşturun

Blob Depolama hesabınızda aşağıdaki adımları uygulayarak öğretici için hazırlama.

1. Adlı bir kapsayıcı oluşturun **kaynak**.  Bir klasör yolu olarak oluşturma **2019/02/26/14** kapsayıcınızda. Boş bir metin dosyası oluşturun ve olarak adlandırın **Dosya1.ref**. Klasör yoluna Dosya1.ref karşıya **kaynak/2019/02/26/14** depolama hesabınızda.  Bu görevleri [Azure Depolama Gezgini](https://storageexplorer.com/) gibi çeşitli araçlar kullanarak gerçekleştirebilirsiniz.
    
    ![Dosyaları karşıya yükleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/upload-file.png)
    
    > [!NOTE]
    > Lütfen bir klasör adı, UTC saati ile ayarlayın.  Örneğin, geçerli UTC saat 2: 03'te 26 Şubat 2019 tarihinde ise klasör yolu olarak oluşturabilirsiniz **kaynak/2019/02/26/14/** kuralına göre **kaynak / {Year} / {Month} / {Day} / {Hour} /**.

2. Adlı bir kapsayıcı oluşturun **hedef**. Bu görevleri [Azure Depolama Gezgini](https://storageexplorer.com/) gibi çeşitli araçlar kullanarak gerçekleştirebilirsiniz.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüden **kaynak Oluştur** > **veri ve analiz** > **Data Factory**: 
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin. 
      
    ![Yeni veri fabrikası](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
    
    Veri fabrikanızın adı _genel olarak benzersiz_ olmalıdır. Aşağıdaki hata iletisini alabilirsiniz:
   
   ![Yeni veri fabrikası hata iletisi](./media/tutorial-copy-data-tool/name-not-available-error.png)
   
   Ad değeriyle ilgili bir hata iletisi alırsanız, veri fabrikası için farklı bir ad girin. Örneğin, _**adınız**_**ADFTutorialDataFactory** adını kullanın. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
3. Yeni veri fabrikasının oluşturulacağı Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
    a. **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

    b. **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin. 
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).

5. **Sürüm** bölümünde **V2**'yi seçin.
6. **Konum** bölümünde veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikanız tarafından kullanılan veri depoları (örneğin, Azure Depolama ve SQL Veritabanı) ve işlemler (örneğin, Azure HDInsight) başka konumlarda ve bölgelerde olabilir.
7. **Panoya sabitle**’yi seçin. 
8. **Oluştur**’u seçin.
9. Panodaki **Veri Fabrikası Dağıtılıyor** kutucuğu işlem durumunu gösterir.

    ![Veri fabrikası dağıtılıyor kutucuğu](media/tutorial-copy-data-tool/deploying-data-factory.png)
10. Oluşturma işlemi tamamlandıktan sonra **Data Factory** giriş sayfası görüntülenir.
   
    ![Data factory giriş sayfası](./media/tutorial-copy-data-tool/data-factory-home-page.png)
11. Azure Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Yazar ve İzleyici** kutucuğunu seçin. 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Veri Kopyalama aracını kullanarak işlem hattı oluşturma

1. Üzerinde **başlayalım** sayfasında **veri kopyalama** veri kopyalama aracını başlatmak için başlık. 

   ![Veri Kopyalama aracının kutucuğu](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
   
2. Üzerinde **özellikleri** sayfasında, aşağıdaki adımları uygulayın:

    a. Altında **görev adı**, girin **DeltaCopyFromBlobPipeline**.

    b. Altında **görev temposu veya görev zamanlaması**seçin **düzenli olarak zamanlamaya göre çalıştırma**.

    c. Altında **tetikleme türü**seçin **atlayan pencere**.
    
    d. Altında **yinelenme**, girin **1 saat**. 
    
    e. **İleri**’yi seçin. 
    
    Data Factory kullanıcı arabirimi, belirtilen görev adına sahip bir işlem hattı oluşturur. 

    ![Özellikler sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/copy-data-tool-properties-page.png)
3. **Kaynak veri deposu** sayfasında aşağıdaki adımları tamamlayın:

    a. Tıklayın **+ yeni bağlantı oluştur**, bir bağlantı eklemek için.

    ![Kaynak veri deposu sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page.png)
    
    b. Seçin **Azure Blob Depolama** galeri ve ardından **devam**.

    ![Kaynak veri deposu sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-select-blob.png)
    
    c. Üzerinde **yeni bağlı hizmet** sayfasında, depolama hesabınızdan **depolama hesabı adı** listeleyin ve ardından **son**.
    
    ![Kaynak veri deposu sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-linkedservice.png)
    
    d. Yeni oluşturulan bağlı hizmeti seçin ve ardından tıklayın **sonraki**. 
    
   ![Kaynak veri deposu sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-select-linkedservice.png)
4. **Girdi dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:
    
    a. Göz atın ve seçim **kaynak** kapsayıcı, ardından **Seç**.
    
    ![Girdi dosyası veya klasörü seçin](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-input-file-folder.png)
    
    b. Altında **dosya yükleme davranışını**seçin **artımlı yükleme: klasör/dosya zaman bölümlenmiş adları**.
    
    ![Girdi dosyası veya klasörü seçin](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-loading-behavior.png)
    
    c. Dinamik klasör yolu olarak yazma **kaynak / {year} / {month} / {day} / {hour} /** ve biçimini aşağıdakilere değiştirin:
    
    ![Girdi dosyası veya klasörü seçin](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/input-file-name.png)
    
    d. Denetleme **ikili kopya** tıklatıp **sonraki**.
    
    ![Girdi dosyası veya klasörü seçin](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/check-binary-copy.png)     
5. Üzerinde **hedef veri deposuna** sayfasında **AzureBlobStorage**, aynı depolama alanı olan veri kaynağı deposu hesap ve ardından **sonraki**.

    ![Hedef veri deposu sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/destination-data-store-page-select-linkedservice.png) 
6. Üzerinde **çıktı dosyasını veya klasörünü seçin** sayfasında, aşağıdaki adımları uygulayın:
    
    a. Göz atın ve seçim **hedef** klasörü, ardından **Seç**.
    
    ![Çıktı dosyasını veya klasörünü seçin](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-output-file-folder.png)   
    
    b. Dinamik klasör yolu olarak yazma **kaynak / {year} / {month} / {day} / {hour} /** ve biçimini aşağıdakilere değiştirin:
    
    ![Çıktı dosyasını veya klasörünü seçin](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/input-file-name2.png)    
    
    c. **İleri**’ye tıklayın.
    
    ![Çıktı dosyasını veya klasörünü seçin](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/click-next-after-output-folder.png)  
7. **Ayarlar** sayfasında **İleri**’yi seçin. 

    ![Ayarlar sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/settings-page.png)  
8. **Özet** sayfasında ayarları gözden geçirin ve **İleri**’yi seçin.

    ![Özet sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/summary-page.png)
    
9. **Dağıtım** sayfasında, işlem hattını (görev) izlemek için **İzleyici**’yi seçin.
    ![Dağıtım sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/deployment-page.png)
    
10. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin.  Otomatik olarak tetiklendiğinde hattını beklemeniz gerekir (hakkında bir saat sonra).  Çalıştığında **eylemleri** sütununda etkinlik çalıştırması ayrıntılarını görüntüleme ve işlem hattını yeniden çalıştırma bağlantılarını içerir. Seçin **Yenile** Listeyi yenileyin ve **etkinlik çalıştırmalarını görüntüle** bağlantısını **eylemleri** sütun. 

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs1.png)
11. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. Kaynak dosyası (Dosya1.ref) öğesinden kopyalandı gördüğünüz **kaynak/2019/02/26/14/** için **hedef/2019/02/26/14/** aynı dosya adına sahip.  

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs2.png)
    
    Azure Depolama Gezgini'ni kullanarak aynı da doğrulayabilirsiniz (https://storageexplorer.com/) dosyaları taramak için.
    
    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs3.png)
12. Yeni ada sahip başka bir boş metin dosyası oluşturun **file2.txt**. Klasör yoluna file2.txt dosyayı karşıya yüklemeyi **kaynak/2019/02/26/15** depolama hesabınızda.   Bu görevleri [Azure Depolama Gezgini](https://storageexplorer.com/) gibi çeşitli araçlar kullanarak gerçekleştirebilirsiniz.   
    
    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs4.png)
    
    > [!NOTE]
    > Yeni bir klasör yolu oluşturulması için gerekli olduğunu unutmayın. Lütfen bir klasör adı, UTC saati ile ayarlayın.  Örneğin, geçerli UTC saati 3:20 üzerinde 26 Şubat 2019'te ise klasör yolu olarak oluşturabilirsiniz **kaynak/2019/02/26/15/** kuralına göre **{Year} / {Month} / {Day} / {Hour} /**.
    
13. Dönmek için **işlem hattı çalıştırmaları** görüntülenecek **tüm işlem hattı çalıştırmaları**, yeniden tetiklenmesi için aynı işlem hattı bekleyin başka bir saat sonra otomatik olarak.  

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs5.png)

14. Seçin **etkinlik çalıştırma görünümü** ne zaman ikinci işlem hattı için gelir ve ayrıntılarını gözden geçirmek için de aynısını yapın.  

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs6.png)
    
    Kaynak dosyası (file2.txt) öğesinden kopyalandı gördüğünüz **kaynak/2019/02/26/15/** için **hedef/2019/02/26/15/** aynı dosya adına sahip.
    
    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs7.png) 
    
    Azure Depolama Gezgini'ni kullanarak aynı da doğrulayabilirsiniz (https://storageexplorer.com/) dosyaları taramak için **hedef** kapsayıcı
    
    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs8.png)

    
## <a name="next-steps"></a>Sonraki adımlar
Azure üzerinde bir Spark kümesi kullanarak veri dönüştürme hakkında bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Bulutta Spark kümesi kullanarak verileri dönüştürme](tutorial-transform-data-spark-portal.md)