---
title: "Azure Data Factory UI kullanarak Azure veri fabrikası oluşturma | Microsoft Docs"
description: "Bu öğreticide, Azure Blob Depolama Alanı'nda bir klasördeki verileri başka bir klasöre kopyalayan bir işlem hattıyla veri fabrikası oluşturma işlemi gösterilir."
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
ms.openlocfilehash: ebce4e0d137d2e56cc914efad357593af7abb255
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-data-factory-using-the-azure-data-factory-ui"></a>Azure Data Factory UI kullanarak veri fabrikası oluşturma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Sürüm 2 - Önizleme](quickstart-create-data-factory-portal.md)

Bu hızlı başlangıçta, Azure Data Factory UI kullanarak veri fabrikasını oluşturma ve izleme işlemi açıklanır. Bu veri fabrikasında oluşturduğunuz işlem hattı, verileri Azure blob depolama alanındaki bir klasörden başka bir klasöre **kopyalar**. Azure Data Factory kullanarak verileri **dönüştürme** hakkında bir öğretici için bkz. [Öğretici: Spark kullanarak verileri dönüştürme](tutorial-transform-data-spark-portal.md). 


> [!NOTE]
> Azure Data Factory'yi kullanmaya yeni başlıyorsanız, bu hızlı başlangıçtaki işlemleri gerçekleştirmeden önce [Azure Data Factory'ye giriş](data-factory-introduction.md) konusuna bakın. 
>
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory'nin genel kullanıma açık 1. sürümünü kullanıyorsanız bkz. [Data Factory sürüm 1 - öğretici](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### <a name="video"></a>Video 
Bu videoyu izlemeniz, Data Factory kullanıcı arabirimini anlamanıza yardımcı olur: 
>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Visually-build-pipelines-for-Azure-Data-Factory-v2/Player]

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. [Azure portalına](https://portal.azure.com) gidin. 
2. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **ad** için **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası sayfası](./media/quickstart-create-data-factory-portal/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
     ![Ad yok - hata](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Açılan listede yalnızca Data Factory tarafından desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları (Azure Depolama, Azure SQL Veritabanı, vb.) ve işlemler (HDInsight, vb.) başka konumlarda olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media//quickstart-create-data-factory-portal/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
    ![Data factory giriş sayfası](./media/quickstart-create-data-factory-portal/data-factory-home-page.png)
10. Azure Data Factory kullanıcı arabirimi (UI) uygulamasını ayrı bir sekmede açmak için **Yazar ve İzleyici** kutucuğuna tıklayın. 
11. Başlarken sayfasında, aşağıdaki resimde gösterildiği gibi sol bölmede bulunan **Düzenle** sekmesine geçin: 

    ![Başlarken sayfası](./media/quickstart-create-data-factory-portal/get-started-page.png)

## <a name="create-azure-storage-linked-service"></a>Azure Storage bağlı hizmeti oluşturma
Bu adımda, Azure depolama hesabınızı veri fabrikasına bağlamak için bağlı bir hizmet oluşturursunuz. Bağlı hizmetler, Data Factory hizmetinin bunlara bağlanmak için çalışma zamanında kullandığı bağlantı bilgilerini içerir.

2. **Bağlantılar**'a tıklayın ve sonra da araç çubuğunda **İleri** düğmesine tıklayın. 

    ![Yeni bağlantı](./media/quickstart-create-data-factory-portal/new-connection-button.png)    
3. **Yeni Bağlı Hizmet** sayfasında **Azure Blob Depolama Alanı**’nı seçin ve **Devam**’a tıklayın. 

    ![Azure Depolama Alanı'nı seçin](./media/quickstart-create-data-factory-portal/select-azure-storage.png)
4. **Yeni Bağlı Hizmet** sayfasında aşağıdaki adımları izleyin: 

    1. **Ad** olarak **AzureStorageLinkedService** adını girin.
    2. **Depolama hesabı adı** alanı için Azure Depolama Hesabınızın adını seçin.
    3. Data Factory hizmetinin depolama hesabına bağlanabildiğini onaylamak için **Bağlantıyı sına**'ya tıklayın. 
    4. Bağlı hizmeti kaydetmek için **Kaydet**’e tıklayın. 

        ![Azure Depolama Bağlı Hizmeti ayarları](./media/quickstart-create-data-factory-portal/azure-storage-linked-service.png) 
5. Bağlı hizmetler listesinde **AzureStorageLinkedService** öğesini gördüğünüzü onaylayın. 

    ![Listede Azure Depolama bağlı hizmeti](./media/quickstart-create-data-factory-portal/azure-storage-linked-service-in-list.png)

## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu adımda iki veri kümesi oluşturursunuz: **InputDataset** ve **OutputDataset**. Bu veri kümeleri **AzureBlob** türündedir. Bunlar, önceki adımda oluşturduğunuz **Azure Depolama bağlı hizmetine** işaret eder. 

Giriş veri kümesi, giriş klasöründeki kaynak verileri temsil eder. Giriş veri kümesi tanımında, kaynak verileri içeren blob kapsayıcısını (**adftutorial**), klasörü (**input**) ve dosyayı (**emp.txt**) belirtirsiniz. 

Çıkış veri kümesi hedefe kopyalanan verileri temsil eder. Çıkış veri kümesi tanımında, verilerin kopyalandığı blob kapsayıcısını (**adftutorial**), klasörü (**output**) ve dosyayı belirtirsiniz. Her işlem hattı çalıştırmasıyla ilişkilendirilmiş benzersiz bir kimlik vardır ve bu kimliğe **RunId** sistem değişkeni kullanılarak erişilebilir. Çıkış dosyasının adı, işlem hattının çalıştırma kimliği temelinde dinamik olarak belirlenir.   

Bağlı hizmet ayarlarında, kaynak verileri içeren Azure depolama hesabını belirtmiştiniz. Kaynak veri kümesi ayarlarında, kaynak verilerin tam olarak nerede durduğunu (blob kapsayıcısı, klasör ve dosya) belirtirsiniz. Havuz veri kümesi ayarlarında, verilerin nereye kopyalandığını (blob kapsayıcısı, klasör ve dosya) belirtirsiniz. 
 
1. **+ (artı)** düğmesine tıklayın ve **Veri Kümesi**'ni seçin.

    ![Yeni veri kümesi menüsü](./media/quickstart-create-data-factory-portal/new-dataset-menu.png)
2. **Yeni Veri Kümesi** sayfasında **Azure Blob Depolama**’yı seçip **Son**’a tıklayın. 

    ![Azure Blob Depolama Alanı’nı seçin](./media/quickstart-create-data-factory-portal/select-azure-blob-storage.png)
3. Veri kümesinin **Özellikler** penceresinde, **Ad** olarak **InputDataset** girin. 

    ![Veri kümesi genel ayarları](./media/quickstart-create-data-factory-portal/dataset-general-page.png)
4. **Bağlantı** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. Bağlı hizmet olarak **AzureStorageLinkedService** hizmetini seçin. 
    2. **Dosya yolu** için **Gözat** düğmesine tıklayın. 
        ![Giriş dosyası için Gözat](./media/quickstart-create-data-factory-portal/file-path-browse-button.png)
    3. **Dosya veya klasör seçin** penceresinde **adftutorial** kapsayıcısındaki **input** klasörüne gidin, **emp.txt** dosyasını seçin ve **Son**'a tıklayın.

        ![Giriş dosyası için Gözat](./media/quickstart-create-data-factory-portal/choose-file-folder.png)
    4. (isteğe bağlı) Emp.txt dosyasındaki verilerin önizlemesini görmek için **Verileri önizle**'ye tıklayın.     
5. Çıkış veri kümesini oluşturmak için adımları yineleyin.  

    1. Sol bölmedeki **+ (artı)** düğmesine tıklayın ve **Veri kümesi**'ni seçin.
    2. **Yeni Veri Kümesi** sayfasında **Azure Blob Depolama**’yı seçip **Son**’a tıklayın.
    3. Ad olarak **OutputDataset** değerini belirtin.
    4. Klasör olarak **adftutorial/output** girin. Çıkış klasörü yoksa, Kopyalama etkinliği çıkış klasörünü oluşturur. 
    5. Dosya adı olarak `@CONCAT(pipeline().RunId, '.txt')` girin. İşlem hattını her çalıştırdığınızda, işlem hattı çalıştırmasının kendisiyle ilişkilendirilmiş bir benzersiz kimliği olur. İfade, işlem hattının çalıştırma kimliğini **.txt** uzantısıyla birleştirerek çıkış dosyası adını oluşturur. Desteklenen sistem değişkenleri ve ifadelerin listesi için bkz. [Sistem değişkenleri](control-flow-system-variables.md) ve [İfade dili](control-flow-expression-language-functions.md).

        ![Çıkış veri kümesi ayarları](./media/quickstart-create-data-factory-portal/output-dataset-settings.png)

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma 
Bu adımda, giriş ve çıkış veri kümelerini kullanan **Kopyala** etkinliğiyle bir işlem hattı oluşturur ve doğrularsınız. Kopyalama etkinliği, giriş veri kümesi ayarlarında belirtilen dosyadaki verileri çıkış veri kümesi ayarlarında belirtilen dosyaya kopyalar. Giriş veri kümesi yalnızca klasör belirtiyorsa (dosya adı belirtmiyorsa), Kopyalama etkinliği kaynak klasördeki tüm dosyaları hedefe kopyalar. 

1. **+ (artı)** düğmesine tıklayın ve **İşlem Hattı**'nı seçin. 

    ![Yeni işlem hattı menüsü](./media/quickstart-create-data-factory-portal/new-pipeline-menu.png)
2. **Özellikler** penceresinde **Ad** olarak **CopyPipeline** değerini belirtin. 

    ![İşlem hattı genel ayarları](./media/quickstart-create-data-factory-portal/pipeline-general-settings.png)
3. **Etkinlikler** araç kutusunda **Veri Akışı**’nı genişletin ve **Kopyalama** etkinliğini **Etkinlikler** araç kutusundan sürükleyip işlem hattı tasarımcı yüzeyine bırakın. Ayrıca, **Etkinlikler** araç kutusunda etkinlikler için arama yapabilirsiniz. **Ad** olarak**CopyFromBlobToBlob** değerini belirtin.

    ![Kopyalama etkinliği genel ayarları](./media/quickstart-create-data-factory-portal/copy-activity-general-settings.png)
4. Kopyalama etkinliği ayarlarında **Kaynak** sekmesine geçin ve **kaynak veri kümesi** olarak **InputDataset** öğesini seçin.

    ![Kopyalama etkinliği kaynak ayarları](./media/quickstart-create-data-factory-portal/copy-activity-source-settings.png)    
5. Kopyalama etkinliği ayarlarında **Havuz** sekmesine geçin ve **havuz veri kümesi** olarak **OutputDataset** öğesini seçin.

    ![Kopyalama etkinliği havuz ayarları](./media/quickstart-create-data-factory-portal/copy-activity-sink-settings.png)    
7. İşlem hattı ayarlarını doğrulamak için **Doğrula**'ya tıklayın. Bu işlem hattının başarıyla doğrulandığını onaylayın. Doğrulama çıkışını kapatmak için **sağ ok** (>>) düğmesine tıklayın. 

    ![İşlem hattını doğrulama](./media/quickstart-create-data-factory-portal/pipeline-validate-button.png)

## <a name="test-run-the-pipeline"></a>İşlem hattında test çalıştırması yapma
Bu adımda, işlem hattını Data Factory'de dağıtmadan önce test çalıştırması yaparsınız. 

1. İşlem hattının araç çubuğunda **Test Çalıştırması**'na tıklayın. 
    
    ![İşlem hattı test çalışmaları](./media/quickstart-create-data-factory-portal/pipeline-test-run.png)
2. İşlem hattı ayarlarının **Çıkış** sekmesinde işlem hattı çalıştırmasının durumunu gördüğünüzü onaylayın. 

    ![Çıkışta test çalıştırması yapma](./media/quickstart-create-data-factory-portal/test-run-output.png)    
3. **adftutorial** kapsayıcısının **output** klasöründe bir çıkış dosyası gördüğünüzü onaylayın. Çıkış klasörü yoksa, Data Factory hizmeti tarafından otomatik olarak oluşturulur. 
    
    ![Çıkışı doğrulama](./media/quickstart-create-data-factory-portal/verify-output.png)


## <a name="trigger-the-pipeline-manually"></a>İşlem hattını el ile tetikleme
Bu adımda, varlıkları (bağlı hizmetler, veri kümeleri, işlem hatları) Azure Data Factory'ye dağıtırsınız. Ardından, işlem hattı çalıştırmasını el ile tetiklersiniz. Ayrıca varlıkları kendi VSTS GIT deponuzda da yayımlayabilirsiniz. Bu konu [başka bir öğreticinin](tutorial-copy-data-portal.md?#configure-code-repository) kapsamındadır.

1. İşlem hattını tetiklemeden önce varlıkları Data Factory'de yayımlamanız gerekir. Yayımlamak için, sol bölmede **Yayımla**'ya tıklayın. 

    ![Yayımla düğmesi](./media/quickstart-create-data-factory-portal/publish-button.png)
2. İşlem hattını el ile tetiklemek için, araç çubuğunda **Tetikleyici**'ye tıklayın ve **Şimdi Tetikle**'yi seçin. 
    
    ![Şimdi tetikle](./media/quickstart-create-data-factory-portal/pipeline-trigger-now.png)

## <a name="monitor-the-pipeline"></a>İşlem hattını izleme

1. Soldaki **İzleyici** sekmesine geçin. Listeyi yenilemek için **Yenile** düğmesini kullanın.

    ![İşlem hattını izleme](./media/quickstart-create-data-factory-portal/monitor-trigger-now-pipeline.png)
2. **Eylemler**'in altındaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısına tıklayın. Bu sayfada kopyalama etkinliği çalıştırmasının durumunu görürsünüz. 

    ![İşlem hattı etkinliği çalıştırmaları](./media/quickstart-create-data-factory-portal/pipeline-activity-runs.png)
3. Kopyalama işlemiyle ilgili ayrıntıları görüntülemek için **Eylemler** sütunundaki **Ayrıntılar**’a (gözlük resmi) tıklayın. Özelliklerle ilgili ayrıntılar için bkz. [Kopyalama Etkinliğine genel bakış](copy-activity-overview.md). 

    ![Kopyalama işlemi ayrıntıları](./media/quickstart-create-data-factory-portal/copy-operation-details.png)
4. **Çıkış** klasöründe yeni bir dosya gördüğünüzü onaylayın. 
5. **İşlem Hatları** bağlantısına tıklayarak **Etkinlik Çalıştırmaları** görünümünden **İşlem Hattı Çalıştırmaları** görünümüne dönebilirsiniz. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>İşlem hattını bir zamanlamaya göre tetikleme
Bu öğreticide bu adım isteğe bağlıdır. İşlem hattını düzenli aralıklarla (saatlik, günlük, vb.) çalıştırılacak şekilde zamanlamak için bir **zamanlayıcı tetikleyicisi** oluşturabilirsiniz. Bu adımda, belirttiğiniz tarih saat değeri son tarih olarak kullanılacak şekilde son tarihe kadar her dakika çalıştırılacak bir tetikleyici oluşturursunuz. 

1. **Düzen** sekmesine geçin. 

    ![Düzen sekmesine geçin](./media/quickstart-create-data-factory-portal/switch-edit-tab.png)
1. Menüde **Tetikleyici**'ye tıklayın ve sonra da **Yeni/Düzenle**'ye tıklayın. 

    ![Yeni tetikleyici menüsü](./media/quickstart-create-data-factory-portal/new-trigger-menu.png)
2. **Tetikleyici Ekle** sayfasında **Tetikleyici seç...** öğesine ve sonra da **Yeni**'ye tıklayın. 

    ![Tetikleyici ekle - yeni tetikleyici](./media/quickstart-create-data-factory-portal/add-trigger-new-button.png)
3. **Yeni Tetikleyici** sayfasında, **Son** alanı için **Tarihinde** öğesini seçin, geçerli saatten birkaç dakika sonrasını bitiş saati olarak belirtin ve **Uygula**'ya tıklayın. Her işlem hattı çalıştırmasının bir maliyeti vardır, bu nedenle bitiş saati olarak başlangıç saatinden yalnızca birkaç dakika sonrasını belirtin. Bunun aynı güne ait olduğundan emin olun. Öte yandan, yayımlama saatiyle bitiş saati arasında işlem hattının çalıştırılmasına yetecek kadar zaman olduğundan emin olun. Tetikleyici siz çözümü kullanıcı arabiriminde kaydettiğinizde değil ancak Data Factory'de yayımladığınızda devreye girer. 

    ![Tetikleyici ayarları](./media/quickstart-create-data-factory-portal/trigger-settings.png)
4. **Yeni Tetikleyici** sayfasında **Etkinleştirildi** seçeneğini işaretleyin ve **İleri**'ye tıklayın 

    ![Tetikleyici ayarları - İleri düğmesi](./media/quickstart-create-data-factory-portal/trigger-settings-next.png)
5. **Yeni Tetikleyici** sayfasında uyarı iletisini gözden geçirin ve **Son**'a tıklayın.

    ![Tetikleyici ayarları - Son düğmesi](./media/quickstart-create-data-factory-portal/new-trigger-finish.png)
6. Değişiklikleri Data Factory'de yayımlamak için **Yayımla**'ya tıklayın. 

    ![Yayımla düğmesi](./media/quickstart-create-data-factory-portal/publish-2.png)
8. Soldaki **İzleyici** sekmesine geçin. Listeyi yenilemek için **Yenile**’ye tıklayın. Yayımlama saatinden başlayıp bitiş saatine kadar işlem hattının dakikada bir çalıştırıldığını görürsünüz. **Tetikleyen** sütunundaki değerlere dikkat edin. El ile tetikleyici çalıştırması daha önce uyguladığınız bir adıma (**Şimdi Tetikle**) aittir. 

    ![Tetiklenen çalıştırmaları izleme](./media/quickstart-create-data-factory-portal/monitor-triggered-runs.png)
9. **İşlem Hattı Çalıştırmaları**'nın yanındaki aşağı oka tıklayarak **Tetikleyici Çalıştırmaları** görünümüne geçin. 

    ![Tetikleme çalıştırmalarını izleme](./media/quickstart-create-data-factory-portal/monitor-trigger-runs.png)    
10. **Çıkış** klasöründe belirtilen son tarih saate kadar her işlem hattı çalıştırması için bir **çıkış dosyası** oluşturulduğunu onaylayın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure blob depolama alanındaki başka bir konuma kopyalar. Daha fazla senaryoda Data Factory’yi kullanma hakkında bilgi almak için [öğreticileri](tutorial-copy-data-portal.md) okuyun. 