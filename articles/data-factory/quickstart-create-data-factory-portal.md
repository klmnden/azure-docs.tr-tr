---
title: Azure Data Factory UI kullanarak Azure veri fabrikası oluşturma | Microsoft Docs
description: Azure Blob’daki bir konumdan başka bir konuma veri kopyalayan işlem hattı içeren bir veri fabrikası oluşturun.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: quickstart
ms.date: 06/20/2018
ms.author: jingwang
ms.openlocfilehash: 6f5a4e04c0d135e85624b04dbcdcda6b7d15a427
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60315566"
---
# <a name="quickstart-create-a-data-factory-by-using-the-azure-data-factory-ui"></a>Hızlı Başlangıç: Azure Data Factory UI kullanarak veri fabrikası oluşturma

> [!div class="op_single_selector" title1="Select the version of Data Factory service that you are using:"]
> * [Sürüm 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Geçerli sürüm](quickstart-create-data-factory-portal.md)

Bu hızlı başlangıçta, Azure Data Factory UI kullanarak veri fabrikasını oluşturma ve izleme işlemi açıklanır. Bu veri fabrikasında oluşturduğunuz işlem hattı, verileri Azure Blob depolama alanındaki bir klasörden başka bir klasöre *kopyalar*. Hakkında bir öğretici için *dönüştürme* Azure Data Factory kullanarak verileri görmek [Öğreticisi: Spark kullanarak verileri dönüştürme](tutorial-transform-data-spark-portal.md).

> [!NOTE]
> Azure Data Factory'yi kullanmaya yeni başlıyorsanız, bu hızlı başlangıçtaki işlemleri gerçekleştirmeden önce [Azure Data Factory'ye giriş](data-factory-introduction.md) konusuna bakın. 

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### <a name="video"></a>Video 
Bu videoyu izlemeniz, Data Factory kullanıcı arabirimini anlamanıza yardımcı olur: 
>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Visually-build-pipelines-for-Azure-Data-Factory-v2/Player]

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
1. [Azure Portal](https://portal.azure.com) gidin. 
1. Soldaki menüden **Kaynak oluşturun**’u, sonra **Analiz**’i ve ardından **Data Factory**’yi seçin. 
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)
1. **Yeni veri fabrikası** sayfasında **Ad** için **ADFTutorialDataFactory** girin. 
      
   ![“Yeni veri fabrikası” sayfası](./media/quickstart-create-data-factory-portal/new-azure-data-factory.png)
 
   Azure data factory adı *küresel olarak benzersiz* olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin **&lt; adınız&gt;ADFTutorialDataFactory**) ve veri fabrikası oluşturmayı yeniden deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - adlandırma kuralları](naming-rules.md) makalesini inceleyin.
  
   ![Bir ad kullanılamadığında alınan hata](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
1. **Abonelik** için, veri fabrikasını oluşturmak istediğiniz Azure aboneliğini seçin. 
1. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
   - **Var olanı kullan**’ı ve ardından listeden var olan bir kaynak grubunu seçin. 
   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
   Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
1. **Sürüm** bölümünde **V2**'yi seçin.
1. **Konum** için, veri fabrikasının konumunu seçin.

   Listede yalnızca Data Factory tarafından desteklenen ve Azure Data Factory meta verilerinizin depolanacağı konumlar gösterilir. Data Factory tarafından kullanılan veri depolarının (Azure Depolama ve Azure SQL Veritabanı gibi) ve işlemlerin (Azure HDInsight gibi) başka konumlarda veya bölgelerde çalışabileceğini unutmayın.

1. **Oluştur**’u seçin.

1. Oluşturma işlemi tamamlandıktan sonra, **Data Factory** sayfasını görürsünüz. Azure Data Factory kullanıcı arabirimi (UI) uygulamasını ayrı bir sekmede başlatmak için **Yazar ve İzleyici** kutucuğunu seçin.
   
   ![Veri fabrikasının “Yazar ve İzleyici” kutucuğuna sahip ana sayfası](./media/quickstart-create-data-factory-portal/data-factory-home-page.png)
1. **Başlayalım** sayfasında, sol bölmede bulunan **Yazar** sekmesine geçin. 

    ![“Başlayalım” sayfası](./media/quickstart-create-data-factory-portal/get-started-page.png)

## <a name="create-a-linked-service"></a>Bağlı hizmet oluşturma
Bu yordamda, Azure depolama hesabınızı veri fabrikasına bağlamak için bağlı bir hizmet oluşturursunuz. Bağlı hizmetler, Data Factory hizmetinin bunlara bağlanmak için çalışma zamanında kullandığı bağlantı bilgilerini içerir.

1. **Bağlantılar**'ı seçip araç çubuğundan **İleri** düğmesini seçin. 

   ![Yeni bağlantı oluşturmaya yönelik düğmeler](./media/quickstart-create-data-factory-portal/new-connection-button.png)    
1. **Yeni Bağlı Hizmet** sayfasında **Azure Blob Depolama**’yı seçip **Devam**’ı seçin. 

   !["Azure Blob Depolama" kutucuğunu seçme](./media/quickstart-create-data-factory-portal/select-azure-blob-linked-service.png)
1. Aşağıdaki adımları tamamlayın: 

   a. **Ad** için **AzureStorageLinkedService** adını girin.

   b. **Depolama hesabı adı** alanı için Azure depolama hesabınızın adını seçin.

   c. Data Factory hizmetinin depolama hesabına bağlanabildiğini onaylamak için **Bağlantıyı sına**'yı seçin. 

   d. Bağlı hizmeti kaydetmek için **Son**’u seçin. 

   ![Azure Depolama bağlı hizmeti ayarları](./media/quickstart-create-data-factory-portal/azure-storage-linked-service.png) 

## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu yordamda, iki veri kümesi oluşturursunuz: **Inputdataset** ve **OutputDataset**. Bu veri kümeleri **AzureBlob** türündedir. Bunlar, önceki bölümde oluşturduğunuz Azure Depolama bağlı hizmetine başvurur. 

Giriş veri kümesi, giriş klasöründeki kaynak verileri temsil eder. Giriş veri kümesi tanımında, kaynak verileri içeren blob kapsayıcısını (**adftutorial**), klasörü (**input**) ve dosyayı (**emp.txt**) belirtirsiniz. 

Çıkış veri kümesi hedefe kopyalanan verileri temsil eder. Çıkış veri kümesi tanımında, verilerin kopyalandığı blob kapsayıcısını (**adftutorial**), klasörü (**output**) ve dosyayı belirtirsiniz. Bir işlem hattının her çalıştırmasıyla ilişkili benzersiz bir Kimlik vardır. Bu kimliğe **RunId** sistem değişkenini kullanarak erişebilirsiniz. Çıkış dosyasının adı, işlem hattının çalıştırma kimliği temelinde dinamik olarak belirlenir.   

Bağlı hizmet ayarlarında, kaynak verileri içeren Azure depolama hesabını belirtmiştiniz. Kaynak veri kümesi ayarlarında, kaynak verilerin tam olarak nerede durduğunu (blob kapsayıcısı, klasör ve dosya) belirtirsiniz. Havuz veri kümesi ayarlarında, verilerin nereye kopyalandığını (blob kapsayıcısı, klasör ve dosya) belirtirsiniz. 
 
1. **+** (artı) düğmesini seçip **Veri Kümesi**'ni seçin.

   ![Veri kümesi oluşturma menüsü](./media/quickstart-create-data-factory-portal/new-dataset-menu.png)
1. **Yeni Veri Kümesi** sayfasında **Azure Blob Depolama**’yı seçip **Son**’u seçin. 

   ![“Azure Blob Depolama”yı seçme](./media/quickstart-create-data-factory-portal/select-azure-blob-dataset.png)
1. Veri kümesinin **Genel** sekmesinde, **Ad** olarak **InputDataset** girin. 

1. **Bağlantı** sekmesine geçin ve aşağıdaki adımları tamamlayın: 

    a. **Bağlı hizmet** için **AzureStorageLinkedService** hizmetini seçin.

    b. **Dosya yolu** için **Gözat** düğmesini seçin.

    ![“Bağlantı” sekmesi ve “Gözat” düğmesi](./media/quickstart-create-data-factory-portal/file-path-browse-button.png) c. **Dosya veya klasör seçin** penceresinde **adftutorial** kapsayıcısındaki **input** klasörüne göz atın ve **emp.txt** dosyasını seçip **Son**'u seçin.

    ![Giriş dosyası için Gözat](./media/quickstart-create-data-factory-portal/choose-file-folder.png)
    
    d. (isteğe bağlı) Emp.txt dosyasındaki verilerin önizlemesini görmek için **Verileri önizle**'yi seçin.     

1. Çıktı veri kümesini oluşturmak için adımları yineleyin:  

   a. **+** (artı) düğmesini seçip **Veri Kümesi**'ni seçin.

   b. **Yeni Veri Kümesi** sayfasında **Azure Blob Depolama**’yı seçip **Son**’u seçin.

   c. **Genel** tablosunda ad için **OutputDataset** değerini belirtin.

   d. **Bağlantı** sekmesinde bağlantılı hizmet olarak **AzureStorageLinkedService** girişini seçin ve dizin alanına **adftutorial/output** klasörünü girin. **Output** klasörü yoksa, kopyalama etkinliği çalışma zamanında bu klasörü oluşturur.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma 
Bu yordamda, giriş ve çıkış veri kümelerini kullanan kopyalama etkinliğiyle bir işlem hattı oluşturur ve doğrularsınız. Kopyalama etkinliği, giriş veri kümesi ayarlarında belirttiğiniz dosyadaki verileri çıkış veri kümesi ayarlarında belirttiğiniz dosyaya kopyalar. Giriş veri kümesi yalnızca bir klasörü belirtiyorsa (dosya adını belirtmiyorsa), kopyalama etkinliği kaynak klasördeki tüm dosyaları hedefe kopyalar. 

1. **+** (artı) düğmesini seçip **İşlem Hattı**'nı seçin. 

   ![Yeni işlem hattı oluşturma menüsü](./media/quickstart-create-data-factory-portal/new-pipeline-menu.png)
1. **Genel** sekmesinde **Ad** için **CopyPipeline** değerini belirtin. 

1. **Etkinlikler** araç kutusunda **Taşı ve Dönüştür**’ü genişletin. **Etkinlikler** araç kutusundan **Kopyalama** etkinliğini işlem hattı tasarımcısının yüzeyine sürükleyin. Ayrıca, **Etkinlikler** araç kutusunda etkinlikler için arama yapabilirsiniz. **Ad** için **CopyFromBlobToBlob** adını belirtin.

   ![Kopyalama etkinliği genel ayarları](./media/quickstart-create-data-factory-portal/copy-activity-general-settings.png)
1. Kopyalama etkinliği ayarlarında **Kaynak** sekmesine geçin ve **Kaynak Veri Kümesi** olarak **InputDataset** öğesini seçin.

1. Kopyalama etkinliği ayarlarında **Kaynak** sekmesine geçin ve **Havuz Veri Kümesi** olarak **OutputDataset** öğesini seçin.

1. İşlem hattı ayarlarını doğrulamak için işlem hattı araç çubuğunda **Doğrula**'ya tıklayın. İşlem hattının başarıyla doğrulandığını onaylayın. Doğrulama çıktısını kapatmak için **>>** (sağ ok) düğmesini seçin. 

## <a name="debug-the-pipeline"></a>İşlem hattında hata ayıklama
Bu adımda, işlem hattını Data Factory'de dağıtmadan önce hata ayıklama yaparsınız. 

1. Tuvalin üzerindeki işlem hattı araç çubuğunun üzerinde **Hata Ayıkla**'ya tıklayarak test çalıştırması tetikleyin. 
    
1. Alt kısımdaki işlem hattı ayarlarının **Çıktı** sekmesinde işlem hattı çalıştırmasının durumunu gördüğünüzü onaylayın. 

1. **adftutorial** kapsayıcısının **output** klasöründe bir çıkış dosyası gördüğünüzü onaylayın. Çıkış klasörü yoksa, Data Factory hizmeti tarafından otomatik olarak oluşturulur. 

## <a name="trigger-the-pipeline-manually"></a>İşlem hattını el ile tetikleme
Bu yordamda, varlıkları (bağlı hizmetler, veri kümeleri, işlem hatları) Azure Data Factory'ye dağıtırsınız. Ardından, işlem hattı çalıştırmasını el ile tetiklersiniz. 

1. Bir işlem hattını tetiklemeden önce varlıkları Data Factory'de yayımlamanız gerekir. Yayımlamak için üst taraftan **Tümünü Yayımla**'yı seçin. 

   ![Yayımla düğmesi](./media/quickstart-create-data-factory-portal/publish-button.png)
1. İşlem hattını el ile tetiklemek için, işlem hattı araç çubuğunda **Tetikleyici**'yi seçip **Şimdi Tetikle**'yi seçin. 

## <a name="monitor-the-pipeline"></a>İşlem hattını izleme

1. Soldaki **İzleyici** sekmesine geçin. Listeyi yenilemek için **Yenile** düğmesini kullanın.

   ![“Yenile” düğmesini içeren, işlem hattı çalıştırmalarını izleme sekmesi](./media/quickstart-create-data-factory-portal/monitor-trigger-now-pipeline.png)
1. **Eylemler**'in altındaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısını seçin. Bu sayfada kopyalama etkinliği çalıştırmasının durumunu görürsünüz. 

   ![İşlem hattı etkinliği çalıştırmaları](./media/quickstart-create-data-factory-portal/pipeline-activity-runs.png)
1. Kopyalama işlemiyle ilgili ayrıntıları görüntülemek için **Eylemler** sütunundaki **Ayrıntılar**’ı (gözlük resmi) seçin. Özelliklerle ilgili ayrıntılar için bkz. [Kopyalama Etkinliğine genel bakış](copy-activity-overview.md). 

   ![Kopyalama işlemi ayrıntıları](./media/quickstart-create-data-factory-portal/copy-operation-details.png)
1. **Çıkış** klasöründe yeni bir dosya gördüğünüzü onaylayın. 
1. **İşlem Hatları** bağlantısını seçerek **Etkinlik Çalıştırmaları** görünümünden **İşlem Hattı Çalıştırmaları** görünümüne dönebilirsiniz. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>İşlem hattını bir zamanlamaya göre tetikleme
Bu yordamda bu adım isteğe bağlıdır. İşlem hattını düzenli aralıklarla (saatlik, günlük, vb.) çalıştırılacak şekilde zamanlamak için bir *zamanlayıcı tetikleyicisi* oluşturabilirsiniz. Bu yordamda, belirttiğiniz bitiş tarihine ve saatine kadar her dakika çalıştırılacak bir tetikleyici oluşturursunuz. 

1. **Yazar** sekmesine geçin. 

1. İşlem hattınıza gidin, işlem hattı araç çubuğundan **Tetikleyici**'yi ve ardından **Yeni/Düzenle**'yi seçin. 

1. **Tetikleyici Ekle** sayfasında **Tetikleyici seç**’i ve sonra **Yeni**’yi seçin. 

1. **Yeni Tetikleyici** sayfasındaki **Son** bölümünden **Tarihinde** öğesini seçin, bitiş saati olarak geçerli saatten birkaç dakika sonrasını belirtin ve **Uygula**'yı seçin. 

   Her işlem hattı çalıştırmasının bir maliyeti olduğundan, bitiş saati olarak başlangıç saatinden yalnızca birkaç dakika sonrasını belirtin. Bunun aynı güne ait olduğundan emin olun. Öte yandan, yayımlama saatiyle bitiş saati arasında işlem hattının çalıştırılmasına yetecek kadar zaman olduğundan emin olun. Tetikleyici siz çözümü kullanıcı arabiriminde kaydettiğinizde değil ancak Data Factory'de yayımladığınızda devreye girer. 

   ![Tetikleyici ayarları](./media/quickstart-create-data-factory-portal/trigger-settings.png)
1. **Yeni Tetikleyici** sayfasında, **Etkinleştirildi** onay kutusunu ve sonra **İleri**’yi seçin. 

   !["Etkinleştirildi" onay kutusu ve "İleri" düğmesi](./media/quickstart-create-data-factory-portal/trigger-settings-next.png)
1. Uyarı iletisini gözden geçirin ve **Son**’u seçin.

   ![Uyarı ve "Son" düğmesi](./media/quickstart-create-data-factory-portal/new-trigger-finish.png)
1. Değişiklikleri Data Factory'de yayımlamak için **Tümünü Yayımla**'yı seçin. 

1. Soldaki **İzleyici** sekmesine geçin. Listeyi yenilemek için **Yenile**’yi seçin. İşlem hattının yayımlama saatinden bitiş saatine kadar dakikada bir çalıştırıldığını görürsünüz. 

   **Tetikleyen** sütunundaki değerlere dikkat edin. El ile tetikleyici çalıştırması daha önce uyguladığınız bir adıma (**Şimdi Tetikle**) aittir. 

   ![Tetiklenen çalıştırmaların listesi](./media/quickstart-create-data-factory-portal/monitor-triggered-runs.png)
1. **Tetikleyici Çalıştırmaları** görünümüne geçin. 

   ![“Tetikleyici Çalıştırmaları” görünümüne geçme](./media/quickstart-create-data-factory-portal/monitor-trigger-runs.png)    
1. **Çıktı** klasöründe belirtilen son tarih ve saate kadar her işlem hattı çalıştırması için bir çıktı dosyası oluşturulduğunu onaylayın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure Blob depolama alanındaki başka bir konuma kopyalar. Data Factory’yi daha fazla senaryoda kullanma hakkında bilgi almak için [öğreticileri](tutorial-copy-data-portal.md) okuyun. 