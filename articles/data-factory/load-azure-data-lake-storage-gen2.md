---
title: Azure Data Lake Storage Gen2 veri yükleme (Önizleme) Azure Data Factory ile
description: Azure Data Lake Storage Gen2 verileri kopyalamak için Azure Data Factory kullanın (Önizleme)
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: jingwang
ms.openlocfilehash: 961c8dea4dbb6b6600d10b75e84a9a84c34c329b
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036810"
---
# <a name="load-data-into-azure-data-lake-storage-gen2-preview-with-azure-data-factory"></a>Azure Data Lake Storage Gen2 Önizleme Azure Data Factory ile veri yüklemek

[Azure Data Lake Storage Gen2 Önizleme](../storage/data-lake-storage/introduction.md) Protokolü hiyerarşik dosya sistemi ad alanı ve güvenlik özellikleri ile Azure Blob Storage analytics çerçeveleri dayanıklı depolama katmana bağlanmak kolaylaşır ekler. Data Lake Storage Gen2 içinde (Önizleme) nesne depolama tüm niteliklerini kalır bir dosya sistemi arabirimi avantajları eklenirken.

Azure Data Factory, bulut tabanlı tam olarak yönetilen bir veri tümleştirme hizmetidir. Hizmet lake zengin bir şirket içi verilerle doldurmak için kullanabileceğiniz ve bulut tabanlı veri depolar ve zamandan tasarruf de analiz çözümleriniz oluştururken. Tablonun desteklenen bağlayıcılar ayrıntılı bir listesi için bkz [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory teklifleri bir genişleme yönetilen veri taşıma çözüm AzCopy, bir komut satırı veri aksine yardımcı programı aktarın. ADF genişleme mimarisi nedeniyle, bu yüksek verimlilik verileri alma. Ayrıntılar için bkz [kopyalama etkinliği performansının](copy-activity-performance.md).

Bu makalede, veri fabrikası kopya veri aracının verileri yüklemek için nasıl kullanılacağı gösterilmektedir _Amazon Web Hizmetleri S3 hizmet_ içine _Azure Data Lake Storage Gen2_. Diğer veri depolarına türlerinden verileri kopyalamak için benzer adımları izleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği: Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) başlamadan önce.
* Data Lake Storage etkin Gen2 Azure depolama hesabı: depolama hesabınız yoksa, tıklatın [burada](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) oluşturmak için.
* Veri içeren bir S3 demetini AWS hesabıyla: Bu makalede, Amazon S3'ten verileri kopyalamak gösterilmektedir. Benzer adımları izleyerek diğer veri depolarına kullanabilirsiniz.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüden seçin **yeni** > **veri + analiz** > **Data Factory**:
   
   ![Yeni bir veri fabrikası oluşturma](./media/load-azure-data-lake-storage-gen2/new-azure-data-factory-menu.png)
2. İçinde **yeni data factory** sayfasında, aşağıdaki görüntüde gösterilen alanlar için değerleri girin: 
      
   ![Yeni veri fabrikası sayfası](./media/load-azure-data-lake-storage-gen2//new-azure-data-factory.png)
 
    * **Ad**: Azure data factory'nizi için genel benzersiz bir ad girin. Hata alırsanız, "veri fabrikası adı \"LoadADLSDemo\" kullanılamıyor" veri fabrikası için farklı bir ad girin. Örneğin, adı kullanabilirsiniz  _**adınız**_**ADFTutorialDataFactory**. Data factory oluşturmayı yeniden deneyin. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: data factory oluşturulacağı Azure aboneliğinizi seçin. 
    * **Kaynak grubu**: aşağı açılan listeden mevcut bir kaynak grubu seçin ya da seçin **Yeni Oluştur** seçeneği ve bir kaynak grubu adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
    * **Sürüm**: seçin **V2(Preview)**.
    * **Konum**: data factory konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depolarına diğer konumları ve bölgelerde olabilir. 

3. **Oluştur**’u seçin.
4. Oluşturma işlemi tamamlandıktan sonra veri fabrikanıza gidin. Gördüğünüz **Data Factory** aşağıdaki görüntüde gösterildiği gibi giriş sayfası: 
   
   ![Data factory giriş sayfası](./media/load-azure-data-lake-storage-gen2/data-factory-home-page.png)

   Seçin **Yazar & İzleyici** ayrı bir sekmede veri tümleştirme uygulamayı başlatmak için döşeme.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2 içine veri yükleme

1. İçinde **başlama** sayfasında, **veri Kopyala** döşeme veri Kopyala aracını başlatmak için: 

   ![Veri Kopyalama aracının kutucuğu](./media/load-azure-data-lake-storage-gen2/copy-data-tool-tile.png)
2. İçinde **özellikleri** sayfasında, belirtin **CopyFromAmazonS3ToADLS** için **görev adı** alan ve select **sonraki**:

    ![Özellikler sayfası](./media/load-azure-data-lake-storage-gen2/copy-data-tool-properties-page.png)
3. İçinde **kaynak veri deposu** sayfasında, **+ yeni bir bağlantı oluşturmak**:

    ![Kaynak veri deposu sayfası](./media/load-azure-data-lake-storage-gen2/source-data-store-page.png)
    
    Seçin **Amazon S3** bağlayıcı galerisini ve select **devam et**
    
    ![Kaynak veri deposu s3 sayfası](./media/load-azure-data-lake-storage-gen2/source-data-store-page-s3.png)
    
4. İçinde **belirtin Amazon S3 bağlantı** sayfasında, aşağıdaki adımları uygulayın:
   1. Belirtin **erişim anahtarı kimliği** değeri.
   2. Belirtin **gizli erişim anahtar** değeri.
   3. Tıklatın **Bağlantıyı Sına** ayarlarını doğrulamak için seçip **son**.
   
   ![Amazon S3 hesabı belirtin](./media/load-azure-data-lake-storage-gen2/specify-amazon-s3-account.png)
   
   4. Yeni bir bağlantı oluşturulduğunu görürsünüz. **İleri**’yi seçin.
   
5. İçinde **girdi dosyası veya klasörü seçin** sayfasında, üzerinden kopyalamak istediğiniz dosya ve klasör gözatın. Klasör/dosya seçin, **Seç**:

    ![Girdi dosyasını veya klasörünü seçin](./media/load-azure-data-lake-storage-gen2/choose-input-folder.png)

6. Denetleyerek kopyalama davranışını belirtin **kopyalama dosyaları yinelemeli olarak** ve **ikili kopyalama** seçenekleri. Seçin **sonraki**:

    ![Çıkış klasörü belirtin](./media/load-azure-data-lake-storage-gen2/specify-binary-copy.png)
    
7. İçinde **hedef veri deposu** sayfasında, **+ yeni bir bağlantı oluşturun**ve ardından **Azure Data Lake Storage Gen2 (Önizleme)** seçip **devam et** :

    ![Hedef veri deposu sayfası](./media/load-azure-data-lake-storage-gen2/destination-data-storage-page.png)

8. İçinde **belirtin Azure Data Lake Storage bağlantı** sayfasında, aşağıdaki adımları uygulayın:

   1. "Depolama hesabı adı" özellikli hesabından bırakma listede aşağı, Data Lake depolama Gen2 seçin.
   2. **İleri**’yi seçin.
   
   ![Azure Data Lake Store hesabını belirtin](./media/load-azure-data-lake-storage-gen2/specify-adls.png)

9. İçinde **çıktı dosyası veya klasörü seçin** want **copyfroms3** çıkış klasörü adı ' nı seçip olarak **sonraki**: 

    ![Çıkış klasörü belirtin](./media/load-azure-data-lake-storage-gen2/specify-adls-path.png)

10. İçinde **ayarları** sayfasında, **sonraki** varsayılan ayarları kullanmak için:

    ![Ayarlar sayfası](./media/load-azure-data-lake-storage-gen2/copy-settings.png)
11. İçinde **Özet** sayfasında, ayarları gözden geçirin ve seçin **sonraki**:

    ![Özet sayfası](./media/load-azure-data-lake-storage-gen2/copy-summary.png)
12. İçinde **dağıtım sayfası**seçin **İzleyici** işlem hattını izleme:

    ![Dağıtım sayfası](./media/load-azure-data-lake-storage-gen2/deployment-page.png)
13. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemler** sütununu içeren bağlantılar Etkinlik ayrıntıları görüntülemek ve ardışık düzeni yeniden çalıştırmak için:

    ![İşlem hattı çalıştırmalarını izleme](./media/load-azure-data-lake-storage-gen2/monitor-pipeline-runs.png)

14. Çalıştırma ardışık düzen ile ilişkili etkinlik çalışması görüntülemek için seçin **etkinlik görünümü çalışır** bağlamak **Eylemler** sütun. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. Ardışık Düzen dönmeyi görünüm çalışır, seçin **ardışık düzen** üst bağlantı. Listeyi yenilemek için **Yenile**’yi seçin. 

    ![Etkinlik çalıştırmalarını izleme](./media/load-azure-data-lake-storage-gen2/monitor-activity-runs.png)

15. Her kopyalama etkinliği yürütme ayrıntılarını görüntülemek için seçin **ayrıntıları** bağlantı (gözlük görüntü) altında **Eylemler** izleme görünümü etkinliğinde. Veri birimi kaynak sunucudan kopyaladığınız havuz, veri işleme, yürütme adımları karşılık gelen süresiyle ve yapılandırmaları kullanılan gibi ayrıntıları izleyebilir:

    ![Ayrıntılar Çalıştır etkinliğini izleme](./media/load-azure-data-lake-storage-gen2/monitor-activity-run-details.png)

16. Verileri Data Lake Storage Gen2 hesabınızda kopyalandığını doğrulayın.

## <a name="best-practice"></a>En iyi uygulama

Büyük hacimli kopyaladığınızda dosya tabanlı veri deposundan verileri için önerilir:

- Dosyalar 10 TB 20 TB'ye fileset için her bölüm.
- Kaynak veya havuz veri depolarına azaltma önlemek için çok fazla sayıda eşzamanlı kopya çalıştırır tetiklemez. Bir kopya çalıştırmak ve İzleyici işleme başlayın, ardından giderek daha gerektiği gibi ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Kopya etkinliği genel bakış](copy-activity-overview.md)
* [Azure Data Lake Storage Gen2 Bağlayıcısı](connector-azure-data-lake-storage.md)