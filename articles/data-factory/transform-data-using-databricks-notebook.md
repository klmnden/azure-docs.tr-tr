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
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/12/2018
ms.author: abnarain
ms.reviewer: douglasl
ms.openlocfilehash: 78954e2dd00e425d2dfdd81d2c3e386f199f4f8f
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="run-a-databricks-notebook-with-the-databricks-notebook-activity-in-azure-data-factory"></a>Azure Data Factory’de Databricks Not Defteri etkinliği ile bir Databricks not defteri çalıştırma

Bu öğreticide, databricks iş kümesine göre bir Databricks not defteri yürüten Azure Data Factory işlem hattı oluşturmak için Azure portalını kullanırsınız. Bu işlem ayrıca yürütme sırasında Databricks not defterine Azure Data Factory parametrelerini geçirir.

Bu öğreticide aşağıdaki adımları gerçekleştireceksiniz:

  - Veri fabrikası oluşturma.

  - Databricks Not Defteri Etkinliği’ni kullanan bir işlem hattı oluşturun.

  - İşlem hattı çalıştırması tetikleyin.

  - İşlem hattı çalıştırmasını izleme.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

Bu özelliğe yönelik on bir dakikalık bir giriş ve tanıtım için, aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/ingest-prepare-and-transform-using-azure-databricks-and-data-factory/player]

## <a name="prerequisites"></a>Ön koşullar

  - **Azure Databricks çalışma alanı**. [Bir Databricks çalışma alanı oluşturun](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) veya var olanı kullanın. Azure Databricks çalışma alanınızda bir Python not defteri oluşturun. Ardından, not defterini yürütün ve Azure Data Factory kullanarak parametreleri not defterine geçirin.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1.  **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.

2.  Soldaki menüden **Yeni**’yi, sonra **Veri ve Analiz**’i ve ardından **Data Factory**’i seçin.

    ![Yeni bir veri fabrikası oluşturma](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image1.png)

3.  **Yeni veri fabrikası** bölmesinde **Ad** altına **ADFTutorialDataFactory** girin.

    Azure data factory adı *küresel olarak benzersiz* olmalıdır. Aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin. (Örneğin, **\<adınız\>ADFTutorialDataFactory** biçimini kullanın). Data Factory yapıtlarının adlandırma kuralları için [Data Factory - adlandırma kuralları](https://docs.microsoft.com/azure/data-factory/naming-rules) makalesini inceleyin.

    ![Yeni veri fabrikası için bir ad belirtin](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image2.png)

4.  **Abonelik** için, veri fabrikasını oluşturmak istediğiniz Azure aboneliğini seçin.

5.  **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
    
    - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.
    
    - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.

    Bu hızlı başlangıçtaki adımlardan bazıları kaynak grubu için **ADFTutorialResourceGroup** adını kullandığınızı varsayar. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

1.  **Sürüm** için **V2 (Önizleme)** öğesini seçin.

2.  **Konum** için, veri fabrikasının konumunu seçin.

    Data Factory V2 şu anda Doğu ABD, Doğu ABD 2 ve Batı Avrupa bölgelerinde veri fabrikası oluşturmanıza olanak sağlar. Data Factory tarafından kullanılan veri depoları (Azure Depolama ve Azure SQL Veritabanı) ve işlemler (HDInsight gibi) başka bölgelerde olabilir.

3.  **Panoya sabitle**’yi seçin.

4.  **Oluştur**’u seçin.

5.  Panoda, **Data Factory Dağıtılıyor** durumuna sahip aşağıdaki kutucuğu görürsünüz:

    ![](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image3.png)

6.  Oluşturma işlemi tamamlandıktan sonra, **Veri fabrikası** sayfasını görürsünüz. Data Factory kullanıcı arabirimi uygulamasını ayrı bir sekmede başlatmak için **Yazar ve İzleyici** kutucuğunu seçin.

    ![Veri fabrikası UI uygulamasını başlatın](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image4.png)

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma

Bu bölümde bir Databricks bağlı hizmetini yazacaksınız. Bu bağlı hizmet, Databricks kümesine bağlantı bilgilerini içerir:

### <a name="create-an-azure-databricks-linked-service"></a>Azure Databricks bağlı hizmeti oluşturma

1.  **Başlayalım** sayfasında, sol bölmede bulunan **Düzenle** sekmesine geçin.

    ![Yeni bağlı hizmeti düzenleme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image5.png)

2.  Pencerenin alt kısmındaki **Bağlantılar**’ı ve sonra **+ Yeni**’yi seçin.
    
    ![Yeni bağlantı oluşturma](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image6.png)

3.  **Yeni Bağlı Hizmet** penceresinde **Veri Deposu** \> **Azure Databricks**’i ve sonra **Devam**’ı seçin.
    
    ![Databricks bağlı hizmeti belirtme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image7.png)

4.  **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları tamamlayın:
    
    1.  **Ad** için ***AzureDatabricks\_LinkedService*** girin
    
    2.  **Küme** için bir **Yeni Küme** seçin
    
    3.  **Etki Alanı / Bölge** için Azure Databricks çalışma alanınızın bulunduğu bölgeyi seçin.
    
    4.  **Küme düğümü türü** için bu öğreticide **Standart\_D3\_v2** seçeneğini belirleyin.
    
    5.  **Erişim Belirteci**’ni Azure Databricks çalışma alanından oluşturun. Adımları [burada](https://docs.databricks.com/api/latest/authentication.html#generate-token) bulabilirsiniz.
    
    6.  **Küme sürümü** için **4.0 Beta**’yı seçin (en son sürüm)
    
    7.  **Çalışan düğümü sayısı** için **2** girin.
    
    8.  **Son**’u seçin

        ![Bağlı hizmet oluşturmayı tamamlayın](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image8.png)

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1.  **+** (artı) düğmesini ve sonra menüden **İşlem Hattı**'nı seçin.

    ![Yeni işlem hattı oluşturma düğmeleri](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image9.png)

2.  **İşlem hattı** içinde kullanılacak bir **parametre** oluşturun. Daha sonra bu parametreyi Databricks Not Defteri Etkinliği’ne geçireceksiniz. Boş işlem hattında **Parametreler** sekmesine, ardından **Yeni**’ye tıklayın ve '**name**' olarak adlandırın.

    ![Yeni parametre oluşturma](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image10.png)

    ![Name parametresini oluşturma](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image11.png)

3.  **Etkinlikler** araç kutusunda **Databricks**’i genişletin. **Etkinlikler** araç kutusundan **Not Defteri** etkinliğini işlem hattı tasarımcısının yüzeyine sürükleyin.

    ![Not defteri tasarımcı yüzeyine sürükleme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image12.png)

4.  Alt kısımdaki **Databricks** **Not Defteri** etkinlik penceresinin özellikler bölümünde aşağıdaki adımları tamamlayın:

    a. **Ayarlar** sekmesine geçin.

    b. **myAzureDatabricks\_LinkedService** öğesini seçin (önceki yordamda oluşturdunuz).

    c. Bir Databricks **Not Defteri yolu** seçin. Şimdi bir not defteri oluşturup burada yolunu belirtelim. Sonraki birkaç adımı izleyerek Not Defteri Yolunu alın.

       1. Azure Databricks Çalışma Alanını başlatma

       2. Çalışma Alanında **Yeni Klasör** oluşturun ve **adftutorial** olarak adlandırın.

          ![Yeni klasör oluşturma](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image13.png)

       3. [Yeni not defteri oluşturun](https://docs.databricks.com/user-guide/notebooks/index.html#creating-a-notebook) (Python). Bu not defterini **adftutorial** Klasörü altında **mynotebook** olarak adlandırıp **,** **Oluştur**’a tıklayın.

          ![Yeni not defteri oluşturma](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image14.png)

          ![Yeni not defterinin özelliklerini ayarlama](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image15.png)

       4. Yeni oluşturulan "mynotebook" adlı not defterine aşağıdaki kodu ekleyin:

           ```
           # Creating widgets for leveraging parameters, and printing the parameters

           dbutils.widgets.text("input", "","")
           dbutils.widgets.get("input")
           y = getArgument("input")
           print "Param -\'input':"
           print y
           ```

           ![Parametreler için pencere öğeleri oluşturma](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image16.png)

       5. Bu örnekte **Not Defteri Yolu** **/adftutorial/mynotebook** şeklindedir

5.  **Data Factory UI yazma aracına** geri dönün. **Notebook1 Etkinliği** altında **Ayarlar** Sekmesine gidin. 
    
    a.  Not Defteri etkinliğine **Parametre Ekleyin**. Daha önce **işlem hattına** eklediğiniz parametrenin aynısını kullanın.

       ![Parametre ekleme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image17.png)

    b.  Parametreyi **input** olarak adlandırın ve değrri **@pipeline().parameters.name** ifadesi olarak sağlayın.

6.  İşlem hattını doğrulamak için araç çubuğundaki **Doğrula** düğmesini seçin. Doğrulama penceresini kapatmak için **\>\>** (sağ ok) düğmesini seçin.

    ![İşlem hattını doğrulama](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image18.png)

7.  **Tümünü Yayımla**. Data Factory kullanıcı arabirimi, varlıkları (bağlı hizmetler ve işlem hattı) Azure Data Factory hizmetinde yayımlar.

    ![Yeni veri fabrikası varlıklarını yayımlama](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image19.png)

## <a name="trigger-a-pipeline-run"></a>İşlem hattı çalıştırmasını tetikleme

Araç çubuğunda **Tetikleyici**’yi ve sonra **Şimdi Tetikle**’yi seçin.

![Şimdi Tetikle komutunu seçme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image20.png)

**İşlem Hattı Çalıştırma** iletişim kutusu **name** parametresini sorar. Burada parametre olarak **/path/filename** seçeneğini kullanın. **Son**'a tıklayın.

![Name parametreleri için bir değer belirtin](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image21.png)

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1.  **İzleyici** sekmesine geçin. Bir işlem hattı çalıştırması gördüğünüzü onaylayın. Not defterinin yürütüldüğü bir Databricks iş kümesinin oluşturulması yaklaşık 5-8 dakika sürer.

    ![İşlem hattını izleme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image22.png)

2.  Düzenli aralıklarla **Yenile**’yi seçerek işlem hattı çalıştırmasının durumunu denetleyin.

3.  İşlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görmek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Göster**’i seçin.

    ![Etkinlik çalıştırmalarını görüntüleme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image23.png)

Üstteki **İşlem Hatları** bağlantısını seçerek işlem hattı çalıştırmaları görünümüne dönebilirsiniz.

## <a name="verify-the-output"></a>Çıktıyı doğrulama

**Azure Databricks çalışma alanında** oturum açabilir, **Kümeler**’e gidebilir ve **İş** durumunu *yürütme bekliyor, çalışıyor veya sonlandırıldı* olarak görebilirsiniz.

![İş kümesini ve işi görüntüleme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image24.png)

**İş adı**’na tıklayıp gezinerek diğer ayrıntıları görebilirsiniz. Çalıştırma başarılı olduğunda, geçirilen parametreleri ve Python not defterinin çıktısını doğrulayabilirsiniz.

![Çalıştırma ayrıntılarını ve çıktıyı görüntüleme](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image25.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekteki işlem hattı bir Databricks Not Defteri etkinliğini tetikler ve Not Defteri’ne bir parametre geçirir. Şunları öğrendiniz:

  - Veri fabrikası oluşturma.

  - Databricks Not Defteri etkinliğini kullanan bir işlem hattı oluşturun.

  - İşlem hattı çalıştırması tetikleyin.

  - İşlem hattı çalıştırmasını izleme.
