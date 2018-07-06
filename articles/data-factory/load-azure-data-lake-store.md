---
title: Veri yükleme ile Azure Data Lake depolama Gen1 Azure Data Factory kullanarak | Microsoft Docs
description: Azure Data Lake depolama Gen1 veri kopyalamak için Azure Data factory'yi kullanın.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: 7cdc4f0ef436fbd7ea3bdf1431b08be3b840290f
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857210"
---
# <a name="load-data-into-azure-data-lake-storage-gen1-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Data Lake depolama Gen1 içine veri yükleme

[Azure Data Lake depolama Gen1](../data-lake-store/data-lake-store-overview.md) (daha önce Azure Data Lake Store da bilinir) bir büyük veri analizi iş yükleri için kuruluş çapında hiper ölçekli depo. Azure Data Lake, herhangi bir boyut, türü ve alma hızı verileri yakalamayı sağlar. Veriler, işletimsel ve keşfe dönük çözümleme için tek bir yerde yakalanır.

Azure Data Factory, tam olarak yönetilen bulut tabanlı veri tümleştirme hizmetidir. Hizmet ile data lake, mevcut sisteminizden doldurmak ve zamandan tasarruf için kullanabileceğiniz analiz çözümlerinizi oluştururken.

Azure Data Factory, verileri Azure Data Lake Store yüklemek için aşağıdaki avantajları sunar:

* **Kolay ayarlama**: sezgisel bir 5 adımlı sihirbazıyla gerekli komut dosyası yok.
* **Zengin veri deposu desteği**: zengin bir şirket içi ve bulut tabanlı veri depoları için yerleşik destek. Tablo ayrıntılı bir listesi için bkz [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
* **Güvenli ve uyumlu**: verileri HTTPS veya ExpressRoute üzerinden aktarılır. Küresel hizmet varlığını verilerinizi coğrafi sınır hiçbir zaman ayrılmaz sağlar.
* **Yüksek performanslı**: 1-GB/sn veri yükleme hızını Azure Data Lake Store kadar. Ayrıntılar için bkz [kopyalama etkinliği performansı](copy-activity-performance.md).

Bu makale, Data Factory-veri kopyalama aracını işlemini göstermektedir _veri yükleme Amazon S3'ten Azure Data Lake Store_. Diğer türlerden veri depolarının veri kopyalamak için benzer adımları izleyebilirsiniz.

> [!NOTE]
> Daha fazla bilgi için [veri kopyalama veya Azure Data Lake Store için Azure Data Factory kullanarak](connector-azure-data-lake-store.md).
## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği: Azure aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) başlamadan önce.
* Azure Data Lake Store: bir Data Lake Store hesabınız yoksa, yönergelere bakın [bir Data Lake Store hesabı oluşturma](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account).
* Amazon S3: Bu makalede, Amazon S3'ten veri kopyalamak nasıl gösterir. Benzer adımları izleyerek diğer veri depolarına kullanabilirsiniz.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüden **yeni** > **veri ve analiz** > **Data Factory**:
   
   ![Yeni bir veri fabrikası oluşturma](./media/load-data-into-azure-data-lake-store/new-azure-data-factory-menu.png)
2. İçinde **yeni veri fabrikası** sayfasında, aşağıdaki görüntüde gösterilen alanlar için değerleri sağlayın: 
      
   ![Yeni veri fabrikası sayfası](./media/load-data-into-azure-data-lake-store//new-azure-data-factory.png)
 
    * **Ad**: Azure data factory'nizi için genel olarak benzersiz bir ad girin. Hatasını alırsanız "veri fabrikası adı \"LoadADLSDemo\" kullanılabilir değil," veri fabrikası için farklı bir ad girin. Örneğin, adı kullanabilirsiniz  _**adınız**_**ADFTutorialDataFactory**. Veri Fabrikası oluşturmayı yeniden deneyin. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: veri fabrikasının oluşturulacağı Azure aboneliğini seçin. 
    * **Kaynak grubu**: aşağı açılan listeden mevcut bir kaynak grubunu seçin ya da seçin **Yeni Oluştur** seçenek ve bir kaynak grubu adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
    * **Sürüm**: seçin **V2**.
    * **Konum**: veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları başka konumlarda ve bölgelerde olabilir. Bu veri depolarına, Azure Data Lake Store, Azure depolama, Azure SQL veritabanı vb. içerir.

3. **Oluştur**’u seçin.
4. Oluşturma işlemi tamamlandıktan sonra veri fabrikanıza gidin. Gördüğünüz **Data Factory** aşağıdaki görüntüde gösterildiği gibi bir giriş sayfası: 
   
   ![Data factory giriş sayfası](./media/load-data-into-azure-data-lake-store/data-factory-home-page.png)

   Seçin **yazar ve İzleyici** veri tümleştirme uygulaması ayrı bir sekmede başlatmak için.

## <a name="load-data-into-azure-data-lake-store"></a>Azure Data Lake Store ile veri yükleme

1. İçinde **başlama** sayfasında **veri kopyalama** veri kopyalama aracını başlatmak için: 

   ![Veri Kopyalama aracının kutucuğu](./media/load-data-into-azure-data-lake-store/copy-data-tool-tile.png)
2. İçinde **özellikleri** sayfasında, belirtin **CopyFromAmazonS3ToADLS** için **görev adı** alan ve seçim **sonraki**:

    ![Özellikler sayfası](./media/load-data-into-azure-data-lake-store/copy-data-tool-properties-page.png)
3. İçinde **kaynak veri deposu** sayfasında **+ yeni bağlantı oluştur**:

    ![Kaynak veri deposu sayfası](./media/load-data-into-azure-data-lake-store/source-data-store-page.png)
    
    Seçin **Amazon S3**seçip **devam et**
    
    ![Kaynak veri deposu s3 sayfası](./media/load-data-into-azure-data-lake-store/source-data-store-page-s3.png)
    
4. İçinde **belirtin Amazon S3 bağlantı** sayfasında, aşağıdaki adımları uygulayın: 
   1. Belirtin **erişim anahtarı kimliği** değeri.
   2. Belirtin **gizli erişim anahtarı** değeri.
   3. **Son**’u seçin.
   
   ![Amazon S3 hesabı belirtin](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account.png)
   
   4. Yeni bir bağlantı görürsünüz. **İleri**’yi seçin.
   
   ![Amazon S3 hesabı belirtin](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account-created.png)
   
5. İçinde **girdi dosyasını veya klasörünü seçin** sayfasında, üzerinden kopyalamak istediğiniz dosya ve klasör gözatın. Klasör/dosya seçin, **Seç**ve ardından **sonraki**:

    ![Girdi dosyasını veya klasörünü seçin](./media/load-data-into-azure-data-lake-store/choose-input-folder.png)

6. Kopyalama davranışını seçerek **dosyaları yinelemeli olarak kopyalama** ve **ikili kopya** (olarak dosya kopyalama-olduğu) seçenekleri. Seçin **sonraki**:

    ![Çıkış klasörü belirtin](./media/load-data-into-azure-data-lake-store/specify-binary-copy.png)
    
7. İçinde **hedef veri deposuna** sayfasında **+ yeni bağlantı oluştur**ve ardından **Azure Data Lake Store**seçip **devam**:

    ![Hedef veri deposu sayfası](./media/load-data-into-azure-data-lake-store/destination-data-storage-page.png)

8. İçinde **belirtin Data Lake Store bağlantısı** sayfasında, aşağıdaki adımları uygulayın: 

   1. İçin Data Lake Store seçin **Data Lake Store hesap adını**.
   2. Belirtin **Kiracı**ve Son'u seçin.
   3. **İleri**’yi seçin.
   
   > [!IMPORTANT]
   > Bu kılavuzda, kullandığınız bir _yönetilen hizmet kimliği_ , Data Lake Store kimlik doğrulaması için. Takip ederek hizmet sorumlusunu Azure Data Lake Store içinde uygun izinleri vermek mutlaka [bu yönergeleri](connector-azure-data-lake-store.md#using-managed-service-identity-authentication).
   
   ![Azure Data Lake Store hesabını belirtin](./media/load-data-into-azure-data-lake-store/specify-adls.png)
9. İçinde **çıktı dosyasını veya klasörünü seçin** want **copyfroms3** çıkış klasörü adı ' nı seçip olarak **sonraki**: 

    ![Çıkış klasörü belirtin](./media/load-data-into-azure-data-lake-store/specify-adls-path.png)

10. İçinde **ayarları** sayfasında **sonraki**:

    ![Ayarlar sayfası](./media/load-data-into-azure-data-lake-store/copy-settings.png)
11. İçinde **özeti** sayfasında, ayarları gözden geçirin ve seçin **sonraki**:

    ![Özet sayfası](./media/load-data-into-azure-data-lake-store/copy-summary.png)
12. İçinde **dağıtım sayfası**seçin **İzleyici** işlem hattını (görev) izlemek için:

    ![Dağıtım sayfası](./media/load-data-into-azure-data-lake-store/deployment-page.png)
13. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemleri** sütununda etkinlik çalıştırması ayrıntılarını görüntüleme ve işlem hattını yeniden çalıştırma bağlantılarını içerir:

    ![İşlem hattı çalıştırmalarını izleme](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)
14. İşlem hattı çalıştırması ile ilişkili etkinlik çalıştırmalarını görüntülemek için seçin **etkinlik çalıştırmalarını görüntüle** bağlantısını **eylemleri** sütun. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. İşlem hattı çalıştırmaları görünümüne dönmek için seçin **işlem hatları** üstündeki bağlantısı. Listeyi yenilemek için **Yenile**’yi seçin. 

    ![Etkinlik çalıştırmalarını izleme](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

15. Her bir kopyalama etkinliği için yürütme ayrıntıları izlemek için **ayrıntıları** altında bağlantı **eylemleri** izleme görünümü etkinlik. Veri kaynağından kopyalanan havuz, veri aktarım hızı, yürütme adımları karşılık gelen süre ve yapılandırmaları kullanılan gibi ayrıntıları izleyebilirsiniz:

    ![İzleyici etkinlik çalışma ayrıntıları](./media/load-data-into-azure-data-lake-store/monitor-activity-run-details.png)

16. Veriler, Azure Data Lake Store kopyalandığını doğrulayın: 

    ![Data Lake Store çıktıyı doğrulama](./media/load-data-into-azure-data-lake-store/adls-copy-result.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Lake Store desteği hakkında bilgi edinmek için şu makaleye geçin: 

> [!div class="nextstepaction"]
>[Azure Data Lake Store Bağlayıcısı](connector-azure-data-lake-store.md)
