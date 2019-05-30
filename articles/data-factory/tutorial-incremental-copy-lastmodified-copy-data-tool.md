---
title: Artımlı olarak veri kopyalama aracını kullanarak LastModifiedDate göre yeni ve değiştirilen dosyaları kopyalama | Microsoft Docs
description: Bir Azure veri fabrikası oluşturun ve ardından yeni dosyaları üzerinde LastModifiedDate göre artımlı olarak yükleme için veri kopyalama aracını kullanın.
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
ms.openlocfilehash: 3865bb10346c4a55adbf94a7df225eacf2c11252
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65519124"
---
# <a name="incrementally-copy-new-and-changed-files-based-on-lastmodifieddate-by-using-the-copy-data-tool"></a>Artımlı olarak veri kopyalama aracını kullanarak LastModifiedDate göre yeni ve değiştirilen dosyaları kopyalama

Bu öğreticide, bir veri fabrikası oluşturmak için Azure portalını kullanacaksınız. Göre yeni ve değiştirilmiş dosyalar yalnızca artımlı olarak kopyalayan bir işlem hattı oluşturmak için veri kopyalama aracını daha sonra kullanacağınız kendi **LastModifiedDate** Azure Blob depolamadan Azure Blob Depolama.

Bunu yaptığınızda, ADF kaynak deposundan tüm dosyaları tarama, dosya filtresi'kendi LastModifiedDate tarafından uygulamak ve yeni ve güncelleştirilmiş dosyayı yalnızca son daraltılmasından için hedef depoyu kopyalayın.  Lütfen ADF tarama büyük miktarlarda dosyaları sağlar, ancak yalnızca birkaç dosyaları kopyalama hedefi için hala dosya tarama nedeniyle uzun süreli de zaman alıcı beklediğiniz olduğunu unutmayın.   

> [!NOTE]
> Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).

Bu öğreticide, aşağıdaki görevleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak bir işlem hattı oluşturun.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Önkoşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**: BLOB Depolama alanı olarak kullanabilmeniz _kaynak_ ve _havuz_ veri deposu. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md) bölümündeki yönergelere bakın.

### <a name="create-two-containers-in-blob-storage"></a>Blob Depolama alanında iki kapsayıcı oluşturun

Blob Depolama hesabınızda aşağıdaki adımları uygulayarak öğretici için hazırlama.

1. Adlı bir kapsayıcı oluşturun **kaynak**. Çeşitli araçları gibi bu görevi gerçekleştirmek için kullanabileceğiniz [Azure Depolama Gezgini](https://storageexplorer.com/).

2. Adlı bir kapsayıcı oluşturun **hedef**. 

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüden **kaynak Oluştur** > **veri ve analiz** > **Data Factory**: 
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
 
   Veri fabrikanızın adı _genel olarak benzersiz_ olmalıdır. Aşağıdaki hata iletisini alabilirsiniz:
   
   ![Yeni veri fabrikası hata iletisi](./media/tutorial-copy-data-tool/name-not-available-error.png)

   Ad değeriyle ilgili bir hata iletisi alırsanız, veri fabrikası için farklı bir ad girin. Örneğin, _**adınız**_ **ADFTutorialDataFactory** adını kullanın. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
3. Azure'ı seçin **abonelik** içinde yeni veri fabrikası oluşturacaksınız. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
    * **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

    * **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin. 
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).

5. Altında **sürüm**seçin **V2**.
6. **Konum** bölümünde veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikanızın kullanan veri depoları (örneğin, Azure depolama ve SQL veritabanı) ve işlemler (örneğin, Azure HDInsight) başka konumlarda ve bölgelerde olabilir.
7. **Panoya sabitle**’yi seçin. 
8. **Oluştur**’u seçin.
9. Panoda başvurmak **Data Factory dağıtılıyor** kutucuğu işlem durumunu görmek için.

    ![Veri Fabrikası dağıtılıyor kutucuğu](media/tutorial-copy-data-tool/deploying-data-factory.png)
10. Oluşturma işlemi tamamlandıktan sonra **Data Factory** giriş sayfası görüntülenir.
   
    ![Data factory giriş sayfası](./media/tutorial-copy-data-tool/data-factory-home-page.png)
11. Azure Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için seçmeniz **yazar ve İzleyici** Döşe. 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Veri Kopyalama aracını kullanarak işlem hattı oluşturma

1. Üzerinde **başlayalım** sayfasında **veri kopyalama** veri kopyalama aracını açmak için başlık. 

   ![Veri Kopyalama aracının kutucuğu](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
   
2. Üzerinde **özellikleri** sayfasında, aşağıdaki adımları uygulayın:

    a. Altında **görev adı**, girin **DeltaCopyFromBlobPipeline**.

    b. Altında **görev temposu** veya **görev zamanlama**seçin **düzenli olarak zamanlamaya göre çalıştırma**.

    c. Altında **tetikleyici türü**seçin **atlayan pencere**.
    
    d. Altında **yinelenme**, girin **15 dakika**. 
    
    e. **İleri**’yi seçin. 
    
    Data Factory kullanıcı arabirimi, belirtilen görev adına sahip bir işlem hattı oluşturur. 

    ![Özellikler sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/copy-data-tool-properties-page.png)
    
3. **Kaynak veri deposu** sayfasında aşağıdaki adımları tamamlayın:

    a. Seçin **+ yeni bağlantı oluştur**, bir bağlantı eklemek için.
    
    ![Kaynak veri deposu sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page.png)

    b. Seçin **Azure Blob Depolama** galeri ve ardından **devam**.
    
    ![Kaynak veri deposu sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-blob.png)

    c. Üzerinde **yeni bağlı hizmet** sayfasında, depolama hesabınızdan **depolama hesabı adı** listeleyin ve ardından **son**.
    
    ![Kaynak veri deposu sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-linkedservice.png)
    
    d. Yeni oluşturulan bağlı hizmeti seçin ve ardından **sonraki**. 
    
   ![Kaynak veri deposu sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-linkedservice.png)

4. **Girdi dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:
    
    a. Göz atın ve seçim **kaynak** klasöre tıklayın ve ardından **Seç**.
    
    ![Girdi dosyası veya klasörü seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-input-file-folder.png)
    
    b. Altında **dosya yükleme davranışını**seçin **artımlı yükleme: LastModifiedDate**.
    
    ![Girdi dosyası veya klasörü seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-loading-behavior.png)
    
    c. Denetleme **ikili kopya** seçip **sonraki**.
    
     ![Girdi dosyası veya klasörü seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/check-binary-copy.png)
     
5. Üzerinde **hedef veri deposuna** sayfasında **AzureBlobStorage**. Bu kaynak veri deposu olarak aynı depolama hesabıdır. Sonra **İleri**’yi seçin.

    ![Hedef veri deposu sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/destination-data-store-page-select-linkedservice.png)
    
6. **Çıktı dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:
    
    a. Göz atın ve seçim **hedef** klasöre tıklayın ve ardından **Seç**.
    
    ![Çıktı dosyasını veya klasörünü seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-output-file-folder.png)
    
    b. **İleri**’yi seçin.
    
     ![Çıktı dosyasını veya klasörünü seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/click-next-after-output-folder.png)
    
7. **Ayarlar** sayfasında **İleri**’yi seçin. 

    ![Ayarlar sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/settings-page.png)
    
8. Üzerinde **özeti** sayfasında, ayarları gözden geçirin ve ardından **sonraki**.

    ![Özet sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/summary-page.png)
    
9. **Dağıtım** sayfasında, işlem hattını (görev) izlemek için **İzleyici**’yi seçin.

    ![Dağıtım sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/deployment-page.png)
    
10. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemler** sütunu, etkinlik çalıştırması ayrıntılarını görüntüleme ve işlem hattını yeniden çalıştırma bağlantılarını içerir. Seçin **Yenile** Listeyi yenileyin ve **etkinlik çalıştırmalarını görüntüle** bağlantısını **eylemleri** sütun. 

    ![Listeyi yenileyin ve etkinlik çalıştırmalarını görüntüle seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs1.png)

11. Olduğundan tek bir etkinliğiniz (kopyalama etkinliği) işlem hattında, yalnızca bir giriş görürsünüz. Kopyalama işlemiyle ilgili ayrıntılar için **Eylemler** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. 

    ![İşlem hattında kopyalama etkinliği olan](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs2.png)
    
    Hiçbir dosyasında olduğundan **kaynak** kapsayıcı, Blob Depolama hesabınızdaki görmezsiniz kopyalanan tüm dosya **hedef** Blob Depolama hesabınızdaki kapsayıcı.
    
    ![Kaynak kapsayıcı ya da hedef kapsayıcı dosyası yok](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3.png)
    
12. Boş bir metin dosyası oluşturun ve adlandırın **Dosya1.ref**. Bu metin dosyasına karşıya **kaynak** depolama hesabınızdaki kapsayıcı. Bu görevleri [Azure Depolama Gezgini](https://storageexplorer.com/) gibi çeşitli araçlar kullanarak gerçekleştirebilirsiniz.   

    ![Dosya1.ref oluşturun ve kaynak kapsayıcıya yükleme](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3-1.png)
    
13. Dönmek için **işlem hattı çalıştırmaları** görüntülenecek **tüm işlem hattı çalıştırmaları**, otomatik olarak yeniden tetiklenmesi aynı işlem hattı için bekleyin.  

    ![Tüm işlem hattı çalıştırmaları seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs4.png)

14. Seçin **etkinlik çalıştırma görünümü** ne zaman ikinci işlem hattı çalıştırması için bunu görürsünüz. Ardından ilk işlem hattı çalıştırması için yaptığınız gibi ayrıntıları gözden geçirin.  

    ![Etkinlik çalıştırma görünümü seçin ve ayrıntıları gözden geçirin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs5.png)

    Bkz biri olacak dosyası (Dosya1.ref) öğesinden kopyalandı **kaynak** kapsayıcıya **hedef** kapsayıcı, Blob Depolama hesabı.
    
    ![Hedef kapsayıcı için kaynak kapsayıcısından Dosya1.ref kopyalandı](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs6.png)
    
15. Başka bir boş metin dosyası oluşturun ve adlandırın **file2.txt**. Bu metin dosyasına karşıya **kaynak** Blob Depolama hesabınızdaki kapsayıcı.   
    
16. Bu ikinci metin dosyasını 13 ve 14 numaralı adımları tekrarlayın. Yalnızca yeni dosya (file2.txt) öğesinden kopyalandığını görürsünüz **kaynak** kapsayıcıya **hedef** sonraki işlem hattı çalıştırması depolama hesabınızdaki kapsayıcı.  
    
    ![Hedef kapsayıcı için kaynak kapsayıcısından File2.txt kopyalandı](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs7.png)

    Kullanarak bu da doğrulayabilirsiniz [Azure Depolama Gezgini](https://storageexplorer.com/) dosyaları taramak için.
    
    ![Azure Depolama Gezgini'ni kullanarak dosyaları tara](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs8.png)

    
## <a name="next-steps"></a>Sonraki adımlar
Azure üzerinde Apache Spark kümesi kullanarak verileri dönüştürme hakkında bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Apache Spark kümesi kullanarak verileri bulutta dönüştürme](tutorial-transform-data-spark-portal.md)