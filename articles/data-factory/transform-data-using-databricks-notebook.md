---
title: Azure Data Factory’de Databricks Not Defteri etkinliği ile bir Databricks Not Defteri çalıştırma
description: Bir Azure veri fabrikasında databricks iş kümesine göre Databricks not defteri çalıştırmak için Databricks Not Defteri Etkinliğini nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/12/2018
ms.author: abnarain
ms.reviewer: douglasl
ms.openlocfilehash: ca208ded54d17c2392d764e4cba67e5f24e03fd3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60655379"
---
# <a name="run-a-databricks-notebook-with-the-databricks-notebook-activity-in-azure-data-factory"></a>Azure Data Factory’de Databricks Not Defteri etkinliği ile bir Databricks not defteri çalıştırma

Bu öğreticide, databricks iş kümesine göre bir Databricks not defteri yürüten Azure Data Factory işlem hattı oluşturmak için Azure portalını kullanırsınız. Bu işlem ayrıca yürütme sırasında Databricks not defterine Azure Data Factory parametrelerini geçirir.

Bu öğreticide aşağıdaki adımları gerçekleştireceksiniz:

  - Veri fabrikası oluşturma.

  - Databricks Not Defteri Etkinliği’ni kullanan bir işlem hattı oluşturun.

  - İşlem hattı çalıştırması tetikleyin.

  - İşlem hattı çalıştırmasını izleme.

Azure aboneliğiniz yoksa başlamadan önce  [ücretsiz bir hesap](https://azure.microsoft.com/free/)  oluşturun.

Bu özelliğe yönelik on bir dakikalık bir giriş ve tanıtım için, aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/ingest-prepare-and-transform-using-azure-databricks-and-data-factory/player]

## <a name="prerequisites"></a>Önkoşullar

  - **Azure Databricks çalışma alanı**. [Bir Databricks çalışma alanı oluşturun](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) veya var olanı kullanın. Azure Databricks çalışma alanınızda bir Python not defteri oluşturun. Ardından, not defterini yürütün ve Azure Data Factory kullanarak parametreleri not defterine geçirin.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1.  Başlatma **Microsoft Edge** veya **Google Chrome** web tarayıcısı. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.

1.  Seçin **kaynak Oluştur** sol menüsünde **Analytics**ve ardından **Data Factory**.

    ![Yeni bir veri fabrikası oluşturma](media/transform-data-using-databricks-notebook/new-azure-data-factory-menu.png)

1.  İçinde **yeni veri fabrikası** bölmesinde girin **ADFTutorialDataFactory** altında **adı**.

    Azure data factory adı olmalıdır *genel olarak benzersiz*. Aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin. (Örneğin, **\<adınız\>ADFTutorialDataFactory**). Data Factory yapıtlarının adlandırma kuralları için bkz: [Data Factory - adlandırma kuralları](https://docs.microsoft.com/azure/data-factory/naming-rules) makalesi.

    ![Yeni veri fabrikası için bir ad belirtin](media/transform-data-using-databricks-notebook/new-azure-data-factory.png)

1.  İçin **abonelik**, veri fabrikasını oluşturmak istediğiniz Azure aboneliğini seçin.

1.  İçin **kaynak grubu**, aşağıdaki adımlardan birini gerçekleştirin:
    
    - Seçin **var olanı kullan** ve aşağı açılan listeden mevcut bir kaynak grubunu seçin.
    
    - Seçin **Yeni Oluştur** ve bir kaynak grubu adını girin.

    Bu hızlı başlangıçtaki adımlardan bazıları adı kullandığınızı varsayar **ADFTutorialResourceGroup** kaynak grubu için. Kaynak grupları hakkında daha fazla bilgi için bkz: [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

1.  İçin **sürüm**seçin **V2**.

1.  İçin **konumu**, veri fabrikasının konumunu seçin.

    Data Factory kullanılabildiği şu anda Azure bölgelerinin listesi için aşağıdaki sayfada faiz ve ardından genişletin bölgeleri seçin **Analytics** bulunacak **Data Factory**: [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/). Data Factory tarafından kullanılan veri depoları (Azure Depolama ve Azure SQL Veritabanı) ve işlemler (HDInsight gibi) başka bölgelerde olabilir.
1.  Seçin **oluşturma**.


1.  Oluşturma işlemi tamamlandıktan sonra gördüğünüz **veri fabrikası** sayfası. Seçin **yazar ve İzleyici** Data Factory kullanıcı Arabirimi uygulamasını ayrı bir sekmede başlatmak için bir kutucuk.

    ![Veri fabrikası UI uygulamasını başlatın](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image4.png)

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma

Bu bölümde bir Databricks bağlı hizmetini yazacaksınız. Bu bağlı hizmet, Databricks kümesine bağlantı bilgilerini içerir:

### <a name="create-an-azure-databricks-linked-service"></a>Azure Databricks bağlı hizmeti oluşturma

1.  Üzerinde **başlayalım** sayfasında, geçiş **Düzenle** sol bölmede sekme.

    ![Yeni bağlı hizmeti düzenleme](media/transform-data-using-databricks-notebook/get-started-page.png)

1.  Seçin **bağlantıları** penceresi tıklayın ve ardından sayfanın alt kısmında **+ yeni**.
    
    ![Yeni bağlantı oluşturma](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image6.png)

1.  İçinde **yeni bağlı hizmet** penceresinde **işlem** \> **Azure Databricks**ve ardından ** Devam**.
    
    ![Databricks bağlı hizmeti belirtme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image7.png)

1.  İçinde **yeni bağlı hizmet** penceresinde aşağıdaki adımları tamamlayın:
    
    1.  **Ad** için ***AzureDatabricks\_LinkedService*** girin
    
    1.  Notebook'unuzu çalıştırmak için uygun **Databricks çalışma alanını** seçin

    1.  **Küme seçin**'i ve ardından **Yeni iş kümesi**'ni seçin
    
    1.  **Etki alanı/Bölge** otomatik olarak doldurulmalıdır

    1.  **Erişim Belirteci**’ni Azure Databricks çalışma alanından oluşturun. Adımları [burada](https://docs.databricks.com/api/latest/authentication.html#generate-token) bulabilirsiniz.

    1.  İçin **küme sürümü**seçin **4.2** (ile Apache Spark 2.3.1, Scala 2.11)

    1.  **Küme düğümü türü** için bu öğreticide **Genel Amaçlı (HDD)** bölümünde **Standart\_D3\_v2** seçin. 
    
    1.  **Çalışanlar** alanına **2** yazın.
    
    1.  **Son**’u seçin

        ![Bağlı hizmet oluşturmayı tamamlayın](media/transform-data-using-databricks-notebook/new-databricks-linkedservice.png)

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1.  Seçin **+** (artı) düğmesini ve ardından **işlem hattı** menüsünde.

    ![Yeni işlem hattı oluşturma düğmeleri](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image9.png)

1.  **İşlem hattı** içinde kullanılacak bir **parametre** oluşturun. Daha sonra bu parametreyi Databricks Not Defteri Etkinliği’ne geçireceksiniz. Boş işlem hattında **Parametreler** sekmesine, ardından **Yeni**’ye tıklayın ve '**name**' olarak adlandırın.

    ![Yeni parametre oluşturma](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image10.png)

    ![Name parametresini oluşturma](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image11.png)

1.  İçinde **etkinlikleri** araç genişletin **Databricks**. Sürükleme **not defteri** etkinliğinden **etkinlikleri** işlem hattı Tasarımcı yüzeyine.

    ![Not defteri tasarımcı yüzeyine sürükleme](media/transform-data-using-databricks-notebook/new-adf-pipeline.png)

1.  Özelliklerinde **Databricks** **not defteri** etkinliği penceresinin altındaki, aşağıdaki adımları tamamlayın:

    a. Geçiş **Azure Databricks** sekmesi.

    b. Seçin **AzureDatabricks\_LinkedService** (Bu önceki yordamda oluşturduğunuz).

    c. **Ayarlar** sekmesine geçin

    c. Göz atarak bir Databricks **Not Defteri yolu** seçin. Şimdi bir not defteri oluşturup burada yolunu belirtelim. Sonraki birkaç adımı izleyerek Not Defteri Yolunu alın.

       1. Azure Databricks Çalışma Alanını başlatma

       1. Çalışma Alanında **Yeni Klasör** oluşturun ve **adftutorial** olarak adlandırın.

          ![Yeni klasör oluşturma](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image13.png)

       1. [Yeni bir not defteri oluşturma](https://docs.databricks.com/user-guide/notebooks/index.html#creating-a-notebook) (Python) adlandıralım **mynotebook** altında **adftutorial** klasörünü tıklatın **oluşturun.**

          ![Yeni not defteri oluşturma](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image14.png)

          ![Yeni not defterinin özelliklerini ayarlama](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image15.png)

       1. Yeni oluşturulan "mynotebook" adlı not defterine aşağıdaki kodu ekleyin:

           ```
           # Creating widgets for leveraging parameters, and printing the parameters

           dbutils.widgets.text("input", "","")
           dbutils.widgets.get("input")
           y = getArgument("input")
           print ("Param -\'input':")
           print (y)
           ```

           ![Parametreler için pencere öğeleri oluşturma](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image16.png)

       1. Bu örnekte **Not Defteri Yolu** **/adftutorial/mynotebook** şeklindedir

1.  **Data Factory UI yazma aracına** geri dönün. **Notebook1 Etkinliği** altında **Ayarlar** Sekmesine gidin.

    a.  Not Defteri etkinliğine **Parametre Ekleyin**. Daha önce **işlem hattına** eklediğiniz parametrenin aynısını kullanın.

       ![Parametre ekleme](media/transform-data-using-databricks-notebook/new-adf-parameters.png)

    b.  Parametre olarak ad **giriş** ve ifade olarak değer sağlayın  **\@().parameters.name işlem hattı**.

1.  İşlem hattını doğrulamak için seçin **doğrulama** araç çubuğunda. Doğrulama penceresini kapatmak için seçin **\>\>** (sağ ok) düğmesini.

    ![İşlem hattını doğrulama](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image18.png)

1.  Seçin **tümünü Yayımla**. Data Factory kullanıcı arabirimi, varlıkları (bağlı hizmetler ve işlem hattı) Azure Data Factory hizmetinde yayımlar.

    ![Yeni veri fabrikası varlıklarını yayımlama](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image19.png)

## <a name="trigger-a-pipeline-run"></a>İşlem hattı çalıştırmasını tetikleme

Seçin **tetikleyici** seçin ve araç **şimdi Tetikle**.

![Şimdi Tetikle komutunu seçme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image20.png)

**İşlem Hattı Çalıştırma** iletişim kutusu **name** parametresini sorar. Burada parametre olarak **/path/filename** seçeneğini kullanın. **Son**'a tıklayın.

![Name parametreleri için bir değer belirtin](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image21.png)

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1.  Geçiş **İzleyici** sekmesi. Bir işlem hattı çalıştırması gördüğünüzü onaylayın. Not defterinin yürütüldüğü bir Databricks iş kümesinin oluşturulması yaklaşık 5-8 dakika sürer.

    ![İşlem hattını izleme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image22.png)

1.  Seçin **Yenile** düzenli olarak işlem hattı çalıştırma durumunu denetlemek için.

1.  İşlem hattı çalıştırması ile ilişkili etkinlik çalıştırmalarını görmek için seçin **etkinlik çalıştırmalarını görüntüle** içinde **eylemleri** sütun.

    ![Etkinlik çalıştırmalarını görüntüleme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image23.png)

Seçerek işlem hattı çalıştırmaları görünümüne dönebilirsiniz **işlem hatları** üstündeki bağlantısı.

## <a name="verify-the-output"></a>Çıktıyı doğrulama

**Azure Databricks çalışma alanında** oturum açabilir, **Kümeler**’e gidebilir ve **İş** durumunu *yürütme bekliyor, çalışıyor veya sonlandırıldı* olarak görebilirsiniz.

![İş kümesini ve işi görüntüleme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image24.png)

**İş adı**’na tıklayıp gezinerek diğer ayrıntıları görebilirsiniz. Çalıştırma başarılı olduğunda, geçirilen parametreleri ve Python not defterinin çıktısını doğrulayabilirsiniz.

![Çalıştırma ayrıntılarını ve çıktıyı görüntüleme](media/transform-data-using-databricks-notebook/databricks-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekteki işlem hattı bir Databricks Not Defteri etkinliğini tetikler ve Not Defteri’ne bir parametre geçirir. Şunları öğrendiniz:

  - Veri fabrikası oluşturma.

  - Databricks Not Defteri etkinliğini kullanan bir işlem hattı oluşturun.

  - İşlem hattı çalıştırması tetikleyin.

  - İşlem hattı çalıştırmasını izleme.
