---
title: Azure Data Lake depolama Gen1 verileri Azure Data Factory ile Gen2'ye kopyalayın
description: 2. nesil için Azure Data Lake depolama Gen1 verileri kopyalamak için Azure Data factory'yi kullanın.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: jingwang
ms.openlocfilehash: d6e09ec1f070f9ee0f4162524e4bd80d1f81adc3
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560648"
---
# <a name="copy-data-from-azure-data-lake-storage-gen1-to-gen2-with-azure-data-factory"></a>Azure Data Lake depolama Gen1 verileri Azure Data Factory ile Gen2'ye kopyalayın

Azure Data Lake depolama Gen2 yerleşik, büyük veri analizi için ayrılmış özellikleri kümesidir [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md). Her iki dosya sistemi ve nesne depolama paradigmalarını kullanarak verilerinizi ile arabirim oluşturmasını sağlar.

Azure Data Lake depolama Gen1 kullanıyorsanız, Azure Data Factory kullanarak verileri Data Lake depolama Gen1 Gen2'ye kopyalayarak Gen2 yeni özelliği değerlendirebilirsiniz.

Azure Data Factory, tam olarak yönetilen bulut tabanlı veri tümleştirme hizmetidir. Hizmet lake zengin bir şirket içi verilerle doldurmak için kullanabileceğiniz bulut tabanlı veri depoları ve zamandan tasarruf edin ve analiz çözümlerinizi oluştururken. Desteklenen bağlayıcılar ayrıntılı bir listesi için tablosuna bakın [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory, ölçeği genişletme, yönetilen veri taşıma çözümü sunar. ADF genişleme mimarisi nedeniyle, verileri yüksek bir işleme alın. Ayrıntılar için bkz [kopyalama etkinliği performansı](copy-activity-performance.md).

Bu makalede Data Factory-veri kopyalama aracını veri kopyalamak için nasıl kullanılacağını gösteren _Azure Data Lake depolama Gen1_ içine _Azure Data Lake depolama Gen2_. Diğer türlerden veri depolarının veri kopyalamak için benzer adımları izleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Bu verileri Azure Data Lake depolama Gen1 hesabı.
* Azure depolama hesabı ile Data Lake depolama Gen2'ye etkin: Bir depolama hesabı yoksa [hesap oluşturma](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM).

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüden **kaynak Oluştur** > **veri ve analiz** > **Data Factory**:
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. İçinde **yeni veri fabrikası** sayfasında, aşağıdaki görüntüde gösterilen alanlar için değerleri sağlayın: 
      
   ![Yeni veri fabrikası sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/new-azure-data-factory.png)
 
    * **Ad**: Azure data factory'nizi için genel olarak benzersiz bir ad girin. Hatasını alırsanız "veri fabrikası adı \"LoadADLSDemo\" kullanılabilir değil," veri fabrikası için farklı bir ad girin. Örneğin, adı kullanabilirsiniz  _**adınız**_**ADFTutorialDataFactory**. Veri Fabrikası oluşturmayı yeniden deneyin. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: Veri fabrikasının oluşturulacağı Azure aboneliğini seçin. 
    * **Kaynak grubu**: Aşağı açılan listeden mevcut bir kaynak grubunu seçin ya da seçin **Yeni Oluştur** seçenek ve bir kaynak grubu adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
    * **Sürüm**: Seçin **V2**.
    * **Konum**: Veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları başka konumlarda ve bölgelerde olabilir. 

3. **Oluştur**’u seçin.
4. Oluşturma işlemi tamamlandıktan sonra veri fabrikanıza gidin. Gördüğünüz **Data Factory** aşağıdaki görüntüde gösterildiği gibi bir giriş sayfası: 
   
   ![Data factory giriş sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/data-factory-home-page.png)

   Seçin **yazar ve İzleyici** veri tümleştirme uygulaması ayrı bir sekmede başlatmak için.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2 yük verileri

1. İçinde **başlama** sayfasında **veri kopyalama** veri kopyalama aracını başlatmak için: 

   ![Veri Kopyalama aracının kutucuğu](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-tile.png)
2. İçinde **özellikleri** sayfasında, belirtin **CopyFromADLSGen1ToGen2** için **görev adı** alan ve seçim **sonraki**:

    ![Özellikler sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-properties-page.png)
3. İçinde **kaynak veri deposu** sayfasında **+ yeni bağlantı oluştur**:

    ![Kaynak veri deposu sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page.png)
    
    Seçin **Azure Data Lake depolama Gen1** bağlayıcı galeri ve select **devam et**
    
    ![Azure Data Lake depolama Gen1 sayfasında kaynak veri deposu](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page-adls-gen1.png)
    
4. İçinde **Azure Data Lake depolama Gen1 belirtin bağlantı** sayfasında, aşağıdaki adımları uygulayın:
   1. Hesap adı için Data Lake depolama Gen1 seçip belirtin veya doğrulama **Kiracı**.
   2. Tıklayın **Bağlantıyı Sına** ayarlarını doğrulamak için seçip **son**.
   3. Yeni bir bağlantı oluşturulduğunu görürsünüz. **İleri**’yi seçin.
   
   > [!IMPORTANT]
   > Bu izlenecek yolda, Data Lake depolama Gen1 kimlik doğrulaması için Azure kaynakları için yönetilen bir kimlik kullanın. MSI izleyerek Azure Data Lake depolama Gen1 uygun izinleri vermek mutlaka [bu yönergeleri](connector-azure-data-lake-store.md#managed-identity).
   
   ![Azure Data Lake depolama Gen1 hesabını belirtin](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen1-account.png)
      
5. İçinde **girdi dosyasını veya klasörünü seçin** sayfasında, üzerinden kopyalamak istediğiniz dosya ve klasör gözatın. Klasör/dosya seçin, **Seç**:

    ![Girdi dosyasını veya klasörünü seçin](./media/load-azure-data-lake-storage-gen2-from-gen1/choose-input-folder.png)

6. Kopyalama davranışını kontrol ederek belirtin **dosyaları yinelemeli olarak kopyalama** ve **ikili kopya** seçenekleri. Seçin **sonraki**:

    ![Çıkış klasörü belirtin](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-binary-copy.png)
    
7. İçinde **hedef veri deposuna** sayfasında **+ yeni bağlantı oluştur**ve ardından **Azure Data Lake depolama Gen2'ye**seçip **devam**:

    ![Hedef veri deposu sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/destination-data-storage-page.png)

8. İçinde **Azure Data Lake depolama Gen2 belirtin bağlantı** sayfasında, aşağıdaki adımları uygulayın:

   1. "Depolama hesabı adı" özellikli hesabından açılır listenizi, Data Lake depolama Gen2'ı seçin.
   2. Seçin **son** bağlantı oluşturmak için. Sonra **İleri**’yi seçin.
   
   ![Azure Data Lake depolama Gen2 hesabı belirtin](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-account.png)

9. İçinde **çıktı dosyasını veya klasörünü seçin** want **copyfromadlsgen1** çıkış klasörü adı ' nı seçip olarak **sonraki**. Yoksa ADF kopyalama sırasında karşılık gelen ADLS 2. nesil dosya sistemini ve alt klasör oluşturun.

    ![Çıkış klasörü belirtin](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-path.png)

10. İçinde **ayarları** sayfasında **sonraki** varsayılan ayarları kullanmak için.

11. İçinde **özeti** sayfasında, ayarları gözden geçirin ve seçin **sonraki**:

    ![Özet sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-summary.png)
12. İçinde **dağıtım sayfası**seçin **İzleyici** işlem hattını izlemek için:

    ![Dağıtım sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/deployment-page.png)
13. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemleri** sütununda etkinlik çalıştırması ayrıntılarını görüntüleme ve işlem hattını yeniden çalıştırma bağlantılarını içerir:

    ![İşlem hattı çalıştırmalarını izleme](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-pipeline-runs.png)

14. İşlem hattı çalıştırması ile ilişkili etkinlik çalıştırmalarını görüntülemek için seçin **etkinlik çalıştırmalarını görüntüle** bağlantısını **eylemleri** sütun. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. İşlem hattı çalıştırmaları görünümüne dönmek için seçin **işlem hatları** üstündeki bağlantısı. Listeyi yenilemek için **Yenile**’yi seçin. 

    ![Etkinlik çalıştırmalarını izleme](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-runs.png)

15. Her bir kopyalama etkinliği için yürütme ayrıntıları izlemek için **ayrıntıları** bağlantısını (gözlük resmi) altında **eylemleri** izleme görünümü etkinlik. Veri kaynağından kopyalanan havuz, veri aktarım hızı, yürütme adımları karşılık gelen süre ve yapılandırmaları kullanılan gibi ayrıntıları izleyebilirsiniz:

    ![İzleyici etkinlik çalışma ayrıntıları](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-run-details.png)

16. Verileri Data Lake depolama Gen2 hesabınızda kopyalandığını doğrulayın.

## <a name="best-practices"></a>En iyi yöntemler

Azure Data Lake Storage (ADLS) Gen1 genel için Gen2'ye yükseltme değerlendirmek için başvurmak [büyük veri analiz çözümlerinizi Azure Data Lake depolama 2. nesil için Azure Data Lake depolama Gen1 ' yükseltme](../storage/blobs/data-lake-storage-upgrade.md). Aşağıdaki bölümlerde Gen2 Gen1 veri yükseltme için ADF kullanmanın en iyi yöntemleri tanıtır.

### <a name="data-partition-for-historical-data-copy"></a>Veri bölümü için geçmiş veri kopyalama

- ADLS Gen1, toplam veri boyutu ise kısa **30TB** ve dosya sayısını küçüktür **1 milyon**, tüm veriler tek kopyalama etkinliği çalıştırma kopyalayabilirsiniz.
- Büyük veri kopyalamak için sahip olduğunuz veya toplu veri geçişi yönetmek ve her birinin belirli zamanlama pencereleri içinde tamamlandı esnekliği istediğiniz, verileri bölümlemek için önerilir, bu durumda, ayrıca herhangi bir beklenmeyen ISS riskini azaltabilir UE.

Bir PoC (kavram kanıtı) uçtan uca çözüm doğrulayın ve kopyalama aktarım hızı, ortamınızda test etmek için önerilir. PoC yapmanın ana adımlar: 

1. ADLS Gen1 ADLS Gen2'ile başlayan bir kopyalama performansı temel almak için birkaç TB'a veri kopyalamak için tek bir kopyalama etkinliği ile bir ADF işlem hattı oluşturma [veri tümleştirme birimleri (DIUs)](copy-activity-performance.md#data-integration-units) 128 olarak. 
2. #1. adımda aldığınız kopyalama işleme bağlı olarak, tüm veri geçişi için gereken tahmini süre hesaplayın. 
3. (İsteğe bağlı) Bir denetim tablo oluşturun ve dosyaları geçirilmesi bölümlemek için dosya filtresi tanımlayın. Aşağıdakilere dosyalarla bölüm yolu: 

    - Klasör adı veya klasör adı (önerilen) joker karakter Filtresi ile bölümlenmiş 
    - Dosyanın son değiştirme zamanına göre bölümlenir 

### <a name="network-bandwidth-and-storage-io"></a>Ağ bant genişliği ve depolama g/ç 

Geçiş sırasında ADLS Gen1 normal iş çalışma etkisi olmayan için depolama g/ç kullanımı yönetebilmesi ADLS Gen1 verileri okumak ve veri ADLS Gen2'için yazma ADF kopyası işleri uyumluluğunu denetleyebilirsiniz.

### <a name="permissions"></a>İzinler 

Data factory'de [ADLS Gen1 bağlayıcı](connector-azure-data-lake-store.md) Azure kaynak kimlik doğrulamaları için hizmet sorumlusu ve yönetilen kimliği destekleyen [ADLS Gen2 bağlayıcı](connector-azure-data-lake-storage.md) hizmet sorumlusu ve yönetilen kimliği Azure kaynak kimlik doğrulama için hesap anahtarı destekler. Gidebilirsiniz Data Factory ve kopyalama, gereksinim duyduğunuz kadar tüm dosyalar/ACL yüksek hesabı için yeterli izinleri vermeniz emin olun/okuma/yazma erişimi için tüm dosyaları sağlar ve isterseniz ACL'leri ayarlayın. Geçiş dönemi boyunca super kullanıcıya/sahip rolü olarak vermek için önerilir. 

### <a name="preserve-acls-from-data-lake-storage-gen1"></a>Data Lake depolama Gen1 ACL'sinden koru

ACL veri dosyaları ile birlikte Data Lake depolama Gen1 için Gen2'ye yükseltirken çoğaltmak istiyorsanız, başvurmak [Data Lake depolama Gen1 korumak ACL'sinden](connector-azure-data-lake-storage.md#preserve-acls-from-data-lake-storage-gen1). 

### <a name="incremental-copy"></a>Artımlı kopyalama 

ADLS Gen1 yalnızca yeni veya güncelleştirilmiş dosyaları yüklemek için çeşitli yaklaşımlar kullanılabilir:

- Yeni veya güncelleştirilmiş dosyaları yükleme zaman bölümlenmiş klasör veya dosya adı tarafından örn/2019/05/13 / *;
- Yeni veya güncelleştirilmiş dosyaları LastModifiedDate tarafından yük;
- Herhangi 3 taraf araç/çözümü tarafından yeni veya güncelleştirilmiş dosyaları tanımlayın ve ardından dosya veya klasör adı için ADF işlem hattı parametresi veya bir tablo/dosyası geçirin.  

Artımlı yükleme yapmak için uygun sıklığı ADLS Gen1 dosyalarında toplam sayısı ve her seferinde yüklenecek yeni veya güncelleştirilmiş dosya hacmine bağlıdır.  

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
> [Azure Data Lake depolama Gen1 bağlayıcı](connector-azure-data-lake-store.md)
> [Azure Data Lake depolama Gen2 Bağlayıcısı](connector-azure-data-lake-storage.md)