---
title: Veri yükleme Azure Data Lake Store Azure Data Factory kullanarak | Microsoft Docs
description: Azure Data Factory veri Azure Data Lake Store kopyalamak için kullanın
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
ms.openlocfilehash: fdfb35b0e1c52ad2aad164a38ae308f9142880a6
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34619635"
---
# <a name="load-data-into-azure-data-lake-store-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Data Lake Store içine veri yükleme

[Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) bir büyük veri analitik iş yükleri için kuruluş çapında hiper ölçekli depodur. Azure Data Lake herhangi boyutu, türü ve alım hızına verilerini yakalama olanak sağlar. Verileri tek bir yerde işletimsel ve keşifsel analiz için yakalanır.

Azure Data Factory, bulut tabanlı tam olarak yönetilen bir veri tümleştirme hizmetidir. Hizmet varolan sisteminizden lake verilerle doldurmak ve zamandan tasarruf için kullanabileceğiniz de analiz çözümleriniz oluştururken.

Azure Data Factory verileri Azure Data Lake Store yüklemek için aşağıdaki avantajları sunar:

* **Ayarlamak kolay**: gerekli komut dosyası yok ile sezgisel bir 5-adım Sihirbazı.
* **Zengin veri depolamak Destek**: zengin bir şirket içi ve bulut tabanlı veri depoları için yerleşik destek. Tablonun ayrıntılı bir listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
* **Güvenli ve uyumlu**: veriler HTTPS veya ExpressRoute üzerinden aktarılır. Küresel hizmet varlığı verilerinizi coğrafi sınır asla terk sağlar.
* **Yüksek performanslı**: 1-GB/sn veri hızı Azure Data Lake Store yükleme kadar. Ayrıntılar için bkz [kopyalama etkinliği performansının](copy-activity-performance.md).

Bu makalede, veri fabrikası kopya veri aracının nasıl kullanılacağı gösterilmektedir _veri yükleme Amazon S3'ten Azure Data Lake Store_. Diğer veri depolarına türlerinden verileri kopyalamak için benzer adımları izleyebilirsiniz.

> [!NOTE]
> Daha fazla bilgi için bkz: [veri kopyalama için veya Azure Data Lake Store Azure Data Factory kullanarak](connector-azure-data-lake-store.md).
>
> Bu makale, şu anda önizleme sürümünde olan Azure Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [kopya etkinliği Azure Data factory'de sürüm 1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği: Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) başlamadan önce.
* Azure Data Lake Store: Data Lake Store hesabınız yoksa, deki yönergelere bakın [bir Data Lake Store hesabı oluşturma](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account).
* Amazon S3: Bu makalede Amazon S3'ten veri kopyalama gösterilmektedir. Benzer adımları izleyerek diğer veri depolarına kullanabilirsiniz.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüden seçin **yeni** > **veri + analiz** > **Data Factory**:
   
   ![Yeni bir veri fabrikası oluşturma](./media/load-data-into-azure-data-lake-store/new-azure-data-factory-menu.png)
2. İçinde **yeni data factory** sayfasında, aşağıdaki görüntüde gösterilen alanlar için değerleri girin: 
      
   ![Yeni veri fabrikası sayfası](./media/load-data-into-azure-data-lake-store//new-azure-data-factory.png)
 
    * **Ad**: Azure data factory'nizi için genel benzersiz bir ad girin. Hata alırsanız, "veri fabrikası adı \"LoadADLSDemo\" kullanılamıyor" veri fabrikası için farklı bir ad girin. Örneğin, adı kullanabilirsiniz  _**adınız**_**ADFTutorialDataFactory**. Data factory oluşturmayı yeniden deneyin. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: data factory oluşturulacağı Azure aboneliğinizi seçin. 
    * **Kaynak grubu**: aşağı açılan listeden mevcut bir kaynak grubu seçin ya da seçin **Yeni Oluştur** seçeneği ve bir kaynak grubu adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
    * **Sürüm**: seçin **V2 (Önizleme)**.
    * **Konum**: data factory konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depolarına diğer konumları ve bölgelerde olabilir. Bu veri depolarına Azure Data Lake Store, Azure Storage, Azure SQL Database vb. içerir.

3. **Oluştur**’u seçin.
4. Oluşturma işlemi tamamlandıktan sonra veri fabrikanıza gidin. Gördüğünüz **Data Factory** aşağıdaki görüntüde gösterildiği gibi giriş sayfası: 
   
   ![Data factory giriş sayfası](./media/load-data-into-azure-data-lake-store/data-factory-home-page.png)

   Seçin **Yazar & İzleyici** ayrı bir sekmede veri tümleştirme uygulamayı başlatmak için döşeme.

## <a name="load-data-into-azure-data-lake-store"></a>Azure Data Lake Store içine veri yükleme

1. İçinde **başlama** sayfasında, **veri Kopyala** döşeme veri Kopyala aracını başlatmak için: 

   ![Veri Kopyalama aracının kutucuğu](./media/load-data-into-azure-data-lake-store/copy-data-tool-tile.png)
2. İçinde **özellikleri** sayfasında, belirtin **CopyFromAmazonS3ToADLS** için **görev adı** alan ve select **sonraki**:

    ![Özellikler sayfası](./media/load-data-into-azure-data-lake-store/copy-data-tool-properties-page.png)
3. İçinde **kaynak veri deposu** sayfasında, **Amazon S3**seçip **sonraki**:

    ![Kaynak veri deposu sayfası](./media/load-data-into-azure-data-lake-store/source-data-store-page.png)
4. İçinde **belirtin Amazon S3 bağlantı** sayfasında, aşağıdaki adımları uygulayın: 
   1. Belirtin **erişim anahtarı kimliği** değeri.
   2. Belirtin **gizli erişim anahtar** değeri.
   3. **İleri**’yi seçin.
   
   ![Amazon S3 hesabı belirtin](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account.png)
5. İçinde **girdi dosyası veya klasörü seçin** sayfasında, üzerinden kopyalamak istediğiniz dosya ve klasör gözatın. Klasör/dosya seçin, **Seç**ve ardından **sonraki**:

    ![Girdi dosyasını veya klasörünü seçin](./media/load-data-into-azure-data-lake-store/choose-input-folder.png)

6. İçinde **hedef veri deposu** sayfasında, **Azure Data Lake Store**seçip **sonraki**:

    ![Hedef veri deposu sayfası](./media/load-data-into-azure-data-lake-store/destination-data-storage-page.png)

7. Kopyalama davranışını seçerek **kopyalama dosyaları yinelemeli olarak** ve **ikili kopyalama** (olarak dosyaları kopyalama-olduğu) seçenekleri. Seçin **sonraki**:

    ![Çıkış klasörü belirtin](./media/load-data-into-azure-data-lake-store/specify-binary-copy.png)

8. İçinde **belirt Data Lake Store bağlantısı** sayfasında, aşağıdaki adımları uygulayın: 

   1. Data Lake Store için seçin **Data Lake Store hesabı adı**.
   2. Hizmet asıl bilgileri belirtin: **Kiracı**, **hizmet asıl kimlik**, ve **hizmet asıl anahtarı**.
   3. **İleri**’yi seçin.
   
   > [!IMPORTANT]
   > Bu kılavuzda, kullandığınız bir _hizmet sorumlusu_ Data Lake Store kimliğini doğrulamak için. Hizmet sorumlusu izleyerek Azure Data Lake Store'da uygun izinleri vermek mutlaka [bu yönergeleri](connector-azure-data-lake-store.md#using-service-principal-authentication).
   
   ![Azure Data Lake Store hesabını belirtin](./media/load-data-into-azure-data-lake-store/specify-adls.png)
9. İçinde **çıktı dosyası veya klasörü seçin** want **copyfroms3** çıkış klasörü adı ' nı seçip olarak **sonraki**: 

    ![Çıkış klasörü belirtin](./media/load-data-into-azure-data-lake-store/specify-adls-path.png)

10. İçinde **ayarları** sayfasında, **sonraki**:

    ![Ayarlar sayfası](./media/load-data-into-azure-data-lake-store/copy-settings.png)
11. İçinde **Özet** sayfasında, ayarları gözden geçirin ve seçin **sonraki**:

    ![Özet sayfası](./media/load-data-into-azure-data-lake-store/copy-summary.png)
12. İçinde **dağıtım sayfası**seçin **İzleyici** (görev) işlem hattını izleme:

    ![Dağıtım sayfası](./media/load-data-into-azure-data-lake-store/deployment-page.png)
13. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemler** sütununu içeren bağlantılar Etkinlik ayrıntıları görüntülemek ve ardışık düzeni yeniden çalıştırmak için:

    ![İşlem hattı çalıştırmalarını izleme](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)
14. Çalıştırma ardışık düzen ile ilişkili etkinlik çalışması görüntülemek için seçin **etkinlik görünümü çalışır** bağlamak **Eylemler** sütun. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. Ardışık Düzen dönmeyi görünüm çalışır, seçin **ardışık düzen** üst bağlantı. Listeyi yenilemek için **Yenile**’yi seçin. 

    ![Etkinlik çalıştırmalarını izleme](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

15. Her kopyalama etkinliği yürütme ayrıntılarını görüntülemek için seçin **ayrıntıları** altında bağlantı **Eylemler** izleme görünümü etkinliğinde. Veri birimi kaynak sunucudan kopyaladığınız havuz, veri işleme, yürütme adımları karşılık gelen süresiyle ve yapılandırmaları kullanılan gibi ayrıntıları izleyebilir:

    ![Ayrıntılar Çalıştır etkinliğini izleme](./media/load-data-into-azure-data-lake-store/monitor-activity-run-details.png)

16. Verileri Azure Data Lake deponuza kopyalandığını doğrulayın: 

    ![Data Lake Store çıkış doğrulayın](./media/load-data-into-azure-data-lake-store/adls-copy-result.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Lake Store desteği hakkında bilgi edinmek için aşağıdaki makaleyi ilerletmek: 

> [!div class="nextstepaction"]
>[Azure Data Lake Store Bağlayıcısı](connector-azure-data-lake-store.md)
