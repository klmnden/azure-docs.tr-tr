---
title: "Azure veri Gölü Azure Data Factory kullanarak deposuna veri yükleme | Microsoft Docs"
description: "Azure Data Factory veri Azure Data Lake Store kopyalamak için kullanın"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: 3f73cd65b0ceb3148ce8ceb83d7b4e1be1280077
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="load-data-into-azure-data-lake-store-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Data Lake Store içine veri yükleme

[Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) bir büyük veri analitik iş yükleri için kuruluş çapında hiper ölçekli depodur. Azure Data Lake, işletimsel ve keşifsel analiz için herhangi bir boyuta, türe ve alma hızına sahip olan verileri tek bir konumda yakalamanıza olanak sağlar.

Azure Data Factory lake varolan sisteminizden ve analiz çözümleri oluşturmak, değerli zamandan kaydetme verilerle doldurmak için kullanılan bir bulut tabanlı tam olarak yönetilen bir veri tümleştirme hizmetidir. Azure veri Gölü Azure Data Factory kullanarak deposuna verileri yüklenirken avantajları şunlardır:

* **Ayarlamak kolay**: 5-adım sezgisel Sihirbazı ile gerekli komut dosyası yok.
* **Zengin veri depolamak Destek**: zengin bir şirket içi ve bulut tabanlı veri depoları için yerleşik destek Bkz ayrıntılı listesinde [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.
* **Güvenli ve uyumlu**: veriler HTTPS veya ExpressRoute üzerinden aktarılır ve küresel hizmet varlığı sağlar, verilerinizin hiçbir zaman coğrafi sınır bırakır
* **Yüksek performanslı**: 1 GB/sn veri hızı Azure Data Lake Store yükleme kadar. Ayrıntıları öğrenmek [kopyalama etkinliği performansının](copy-activity-performance.md).

Bu makalede, veri fabrikası kopya veri aracının nasıl kullanılacağı gösterilmektedir **veri yükleme Amazon S3'ten Azure Data Lake Store**. Diğer veri depolarına türlerinden verileri kopyalamak için benzer adımları izleyebilirsiniz.

> [!NOTE]
>  Veri kopyalama/Azure Data Lake Store içinde genel veri fabrikasının özellikleri hakkında bilgi için bkz: [veri kopyalama için veya Azure Data Lake Store Azure Data Factory kullanarak](connector-azure-data-lake-store.md) makalesi.
>
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Önkoşullar

* **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
* **Azure Data Lake Store**. Bir Data Lake Store hesabınız yoksa bkz [bir Data Lake Store hesabı oluşturma](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account) makale oluşturmak adımlar.
* **Amazon S3**. Bu makalede, Amazon S3'ten veri kopyalama gösterilmektedir. Benzer adımları izleyerek diğer veri depolarına kullanabilirsiniz.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüde **YENİ**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/load-data-into-azure-data-lake-store/new-azure-data-factory-menu.png)
2. İçinde **yeni data factory** sayfasında, aşağıdaki ekran görüntüsünde gösterildiği gibi değerlerini belirtin: 
      
     ![Yeni veri fabrikası sayfası](./media/load-data-into-azure-data-lake-store//new-azure-data-factory.png)
 
   * **Adı.** Veri Fabrikası için genel benzersiz bir ad girin. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin adınızADFTutorialDataFactory) ve oluşturmayı yeniden deneyin. Bkz: [Data Factory - adlandırma kurallarını](naming-rules.md) Data Factory yapıtlarının adlandırma kuralları için makale.
  
            `Data factory name "LoadADLSDemo" is not available`

    * **Aboneliği.** Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
    * **Kaynak grubu.** Aşağı açılan listeden mevcut bir kaynak grubu seçin ya da seçin **Yeni Oluştur** seçeneği ve bir kaynak grubu adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
    * **Version.** Seçin **V2 (Önizleme)**.
    * **Konum.** Veri Fabrikası için konum seçin. Yalnızca desteklenen konumlar aşağı açılan listede görüntülenir. Veri fabrikası tarafından kullanılan veri depoları (Azure Data Lake Store, Azure Storage, Azure SQL Database, vb.), diğer konumları/bölgelerde olabilir.

3. **Oluştur**’a tıklayın.
4. Oluşturma tam, veri fabrikası gidin ve gördüğünüz sonra **Data Factory** görüntüde gösterildiği gibi sayfa. Tıklatın **Yazar & İzleyici** ayrı bir sekmede veri tümleştirme uygulamayı başlatmak için döşeme. 
   
   ![Data factory giriş sayfası](./media/load-data-into-azure-data-lake-store/data-factory-home-page.png)

## <a name="load-data-into-azure-data-lake-store"></a>Azure Data Lake Store içine veri yükleme

1. Get başlangıç sayfasını tıklatın **veri Kopyala** veri Kopyala aracını başlatmak için bölme. 

   ![Kopya veri aracı kutucuğu](./media/load-data-into-azure-data-lake-store/copy-data-tool-tile.png)
2. İçinde **özellikleri** sayfasında veri Kopyala aracı belirtin **CopyFromAmazonS3ToADLS** için **görev adı**, tıklatıp **sonraki**. 

    ![Veri aracı-Özellikler sayfası kopyalama](./media/load-data-into-azure-data-lake-store/copy-data-tool-properties-page.png)
3. İçinde **kaynak veri deposu** sayfasında, **Amazon S3**, tıklatıp **sonraki**.

    ![Kaynak veri deposu sayfası](./media/load-data-into-azure-data-lake-store/source-data-store-page.png)
4. İçinde **belirtin Amazon S3 bağlantı** sayfasında, aşağıdaki adımları uygulayın: 
    1. Belirtin **erişim anahtarı kimliği**.
    2. Belirtin **gizli erişim anahtarı**.
    3. **İleri**’ye tıklayın. 

        ![Amazon S3 hesabı belirtin](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account.png)
5. İçinde **girdi dosyası veya klasörü seçin** sayfasında, kopyalayabilir, seçin ve istediğiniz klasör/dosya için gidin **Seç**, ardından **sonraki**. 

    ![Girdi dosyası veya klasörü seçin](./media/load-data-into-azure-data-lake-store/choose-input-folder.png)

6. İçinde **hedef veri deposu** sayfasında, **Azure Data Lake Store**, tıklatıp **sonraki**. 

    ![Hedef veri deposu sayfası](./media/load-data-into-azure-data-lake-store/destination-data-storage-page.png)

7. Bu sayfada denetleyerek kopyalama davranışını seçin **kopyalama dosyaları yinelemeli olarak** ve **ikili kopyalama** (olarak dosyaları kopyalama-olduğu) seçenekleri. **İleri**’ye tıklayın.

    ![Çıkış klasörü belirtin](./media/load-data-into-azure-data-lake-store/specify-binary-copy.png)

8. İçinde **belirt Data Lake Store bağlantısı** sayfasında, aşağıdaki adımları uygulayın: 

    1. Data Lake Store için seçin **Data Lake Store hesabı adı**.
    2. Hizmet asıl bilgiler dahil olmak üzere belirtin **Kiracı**, **hizmet asıl kimlik**, ve **hizmet asıl anahtarı**.
    3. **İleri**’ye tıklayın. 

    > [!IMPORTANT]
    > Bu kılavuzda, kullandığınız bir **hizmet sorumlusu** data lake deposu kimliğini doğrulamak için. Yönergeleri izleyin [burada](connector-azure-data-lake-store.md#using-service-principal-authentication) ve hizmet asıl uygun izni Azure Data Lake Store'da izni olduğundan emin olun.

    ![Azure Data Lake Store hesabını belirtin](./media/load-data-into-azure-data-lake-store/specify-adls.png)

9. İçinde **çıktı dosyası veya klasörü seçin** sayfasında, belirtin **copyfroms3**, ardından **sonraki**. 

    ![Çıkış klasörü belirtin](./media/load-data-into-azure-data-lake-store/specify-adls-path.png)


10. İçinde **ayarları** sayfasında, **sonraki**. 

    ![Ayarları sayfası](./media/load-data-into-azure-data-lake-store/copy-settings.png)
11. İçinde **Özet** sayfasında, ayarları gözden geçirin ve tıklayın **sonraki**.

    ![Özet sayfası](./media/load-data-into-azure-data-lake-store/copy-summary.png)
12. İçinde **dağıtım sayfası**, tıklatın **İzleyici** (görev) işlem hattını izleme.

    ![Dağıtım sayfası](./media/load-data-into-azure-data-lake-store/deployment-page.png)
13. Dikkat **İzleyici** sol sekmesinde otomatik olarak seçilir. Etkinlik ayrıntıları görüntülemek ve ardışık düzeninde yeniden çalıştırmak için bağlantılara bakın **Eylemler** sütun. 

    ![İşlem hattını izleme çalıştırır](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)
14. Etkinlik çalışması çalıştırmak ardışık düzen ile ilişkili görüntülemek için **etkinlik görünümü çalışır** bağlamak **Eylemler** sütun. Olmadığından yalnızca bir etkinlik (kopyalama etkinliği) ardışık düzeninde yalnızca bir girişine bakın. Ardışık Düzen dönmeyi görünüm çalışır, tıklatın **ardışık düzen** üst bağlantı. Tıklatın **yenileme** listesini yenilemek için. 

    ![Monitör etkinliği çalıştırır](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

15. Daha fazla tıklayarak her kopya etkinliğin yürütme ayrıntıları izleyebilirsiniz **ayrıntıları** altında bağlantı **Eylemler** izleme görünümü etkinliğinde. Kaynak havuzu, işleme, ile ilgili süre geçtiği ve yapılandırmaları kullanılan adımları kopyalanan veri hacmi gibi bilgileri gösterir.

    ![Ayrıntılar Çalıştır etkinliğini izleme](./media/load-data-into-azure-data-lake-store/monitor-activity-run-details.png)

16. Verileri Azure Data Lake deponuza kopyalandığını doğrulayın. 

    ![Data Lake Store çıkış doğrulayın](./media/load-data-into-azure-data-lake-store/adls-copy-result.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Lake Store desteği hakkında bilgi edinmek için aşağıdaki makaleyi ilerletmek: 

> [!div class="nextstepaction"]
>[Azure Data Lake Store Bağlayıcısı](connector-azure-data-lake-store.md)