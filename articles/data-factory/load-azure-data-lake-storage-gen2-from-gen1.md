---
title: Azure Data Lake depolama Gen1 verileri Azure Data Factory ile Gen2'ye kopyalayın
description: 2\. nesil için Azure Data Lake depolama Gen1 verileri kopyalamak için Azure Data factory'yi kullanın.
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
ms.openlocfilehash: d13dad6e87bd6c821b497138ad75d7ae2b9a052d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068986"
---
# <a name="copy-data-from-azure-data-lake-storage-gen1-to-gen2-with-azure-data-factory"></a>Azure Data Lake depolama Gen1 verileri Azure Data Factory ile Gen2'ye kopyalayın

Azure Data Lake depolama Gen2 yerleşik olan büyük veri analizi için ayrılmış özellikleri kümesidir [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md). Her iki dosya sistemi ve nesne depolama paradigmalarını kullanarak verilerinizi ile arabirim oluşturmak için kullanabilirsiniz.

Şu anda Azure Data Lake depolama Gen1 kullanıyorsanız, Azure Data Lake depolama Gen2 verileri Data Lake depolama Gen1 Gen2 için Azure Data Factory kullanarak kopyalayarak değerlendirebilirsiniz.

Azure Data Factory, tam olarak yönetilen bulut tabanlı veri tümleştirme hizmetidir. Lake zengin bir şirket içi verilerle doldurmak için hizmeti kullanmak ve bulut tabanlı veri depoları ve analiz çözümlerinizi oluşturduğunuzda zamandan tasarruf edin. Desteklenen bağlayıcılar listesi için tablosuna bakın [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory, ölçeği genişletme, yönetilen veri taşıma çözümü sunar. Data Factory genişleme mimarisi nedeniyle, bu verileri yüksek bir işleme alın. Daha fazla bilgi için [kopyalama etkinliği performansı](copy-activity-performance.md).

Bu makalede Data Factory kopyalama veri aracı verileri Azure Data Lake depolama Gen1 Azure Data Lake depolama Gen2 kopyalamak için nasıl kullanılacağını gösterir. Diğer türlerden veri depolarının veri kopyalamak için benzer adımları izleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Bu verileri Azure Data Lake depolama Gen1 hesabı.
* Data Lake depolama Gen2'ye etkin Azure depolama hesabı. Bir depolama hesabı yoksa [hesap oluşturma](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM).

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüden **kaynak Oluştur** > **veri ve analiz** > **Data Factory**.
   
   ![Yeni bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. Üzerinde **yeni veri fabrikası** sayfasında, aşağıdaki görüntüde gösterilen alanlar için değerleri sağlayın: 
      
   ![Yeni veri fabrikası sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/new-azure-data-factory.png)
 
    * **Ad**: Azure data factory'nizi için genel olarak benzersiz bir ad girin. Hatasını alırsanız "veri fabrikası adı \"LoadADLSDemo\" kullanılabilir değil," veri fabrikası için farklı bir ad girin. Örneğin, _**adınız**_ **ADFTutorialDataFactory** adını kullanın. Veri fabrikasını yeniden oluşturun. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: Veri fabrikasının oluşturulacağı Azure aboneliğini seçin. 
    * **Kaynak grubu**: Aşağı açılan listeden mevcut bir kaynak grubunu seçin. Belirleyebilirsiniz **Yeni Oluştur** seçenek ve bir kaynak grubu adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md). 
    * **Sürüm**: Seçin **V2**.
    * **Konum**: Veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları başka konumlarda ve bölgelerde olabilir. 

3. **Oluştur**’u seçin.
4. Oluşturma işlemi bittikten sonra veri fabrikanıza gidin. Gördüğünüz **Data Factory** aşağıdaki görüntüde gösterildiği gibi bir giriş sayfası: 
   
   ![Data factory giriş sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/data-factory-home-page.png)

5. Seçin **yazar ve İzleyici** veri tümleştirme uygulaması ayrı bir sekmede başlatmak için.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2 yük verileri

1. Üzerinde **başlama** sayfasında **veri kopyalama** veri kopyalama aracını başlatmak için. 

   ![Veri kopyalama aracının kutucuğu](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-tile.png)
2. Üzerinde **özellikleri** sayfasında, belirtin **CopyFromADLSGen1ToGen2** için **görev adı** alan. **İleri**’yi seçin.

    ![Özellikler sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-properties-page.png)
3. Üzerinde **kaynak veri deposu** sayfasında **+ yeni bağlantı oluştur**.

    ![Kaynak veri deposu sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page.png)
    
4. Seçin **Azure Data Lake depolama Gen1** bağlayıcı galeri ve select **devam**.
    
    ![Azure Data Lake depolama Gen1 sayfasında kaynak veri deposu](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page-adls-gen1.png)
    
5. Üzerinde **Azure Data Lake depolama Gen1 belirtin bağlantı** sayfasında, aşağıdaki adımları izleyin:

   a. Hesap adı için Data Lake depolama Gen1 seçip belirtin veya doğrulama **Kiracı**.
  
   b. Seçin **Bağlantıyı Sına** ayarlarını doğrulamak için. Ardından **Son**’u seçin.
  
   c. Yeni bir bağlantı oluşturulduğunu görürsünüz. **İleri**’yi seçin.
   
   > [!IMPORTANT]
   > Bu kılavuzda, Azure Data Lake depolama Gen1 kimlik doğrulaması için Azure kaynakları için yönetilen bir kimlik kullanın. Yönetilen kimlik Azure Data Lake depolama Gen1 uygun izinleri vermek için izleyin [bu yönergeleri](connector-azure-data-lake-store.md#managed-identity).
   
   ![Azure Data Lake depolama Gen1 hesabını belirtin](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen1-account.png)
      
6. Üzerinde **girdi dosyasını veya klasörünü seçin** sayfasında, üzerinden kopyalamak istediğiniz dosya ve klasör gözatın. Klasör veya dosya seçip **Seç**.

    ![Girdi dosyasını veya klasörünü seçin](./media/load-azure-data-lake-storage-gen2-from-gen1/choose-input-folder.png)

7. Kopyalama davranışını belirleyerek **dosyaları yinelemeli olarak kopyalama** ve **ikili kopya** seçenekleri. **İleri**’yi seçin.

    ![Çıkış klasörü belirtin](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-binary-copy.png)
    
8. Üzerinde **hedef veri deposuna** sayfasında **+ yeni bağlantı oluştur** > **Azure Data Lake depolama Gen2**  >   **Devam**.

    ![Hedef veri deposu sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/destination-data-storage-page.png)

9. Üzerinde **Azure Data Lake depolama Gen2 belirtin bağlantı** sayfasında, aşağıdaki adımları izleyin:

   a. Data Lake depolama Gen2 özellikli hesabınızdan seçin **depolama hesabı adı** aşağı açılan listesi.
   
   b. Seçin **son** bağlantı oluşturmak için. Sonra **İleri**’yi seçin.
   
   ![Azure Data Lake depolama Gen2 hesabı belirtin](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-account.png)

10. Üzerinde **çıktı dosyasını veya klasörünü seçin** want **copyfromadlsgen1** çıkış klasörü adı ' nı seçip olarak **sonraki**. Mevcut olmaması durumunda data Factory kopyalama sırasında ilgili Azure Data Lake depolama 2. nesil dosya sistemini ve alt klasörleri oluşturur.

    ![Çıkış klasörü belirtin](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-path.png)

11. Üzerinde **ayarları** sayfasında **sonraki** varsayılan ayarları kullanmak için.

12. Üzerinde **özeti** sayfasında, ayarları gözden geçirin ve seçin **sonraki**.

    ![Özet sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-summary.png)
13. Üzerinde **dağıtım sayfası**seçin **İzleyici** işlem hattını izleme.

    ![Dağıtım sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/deployment-page.png)
14. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemler** sütunu, etkinlik çalıştırması ayrıntılarını görüntüleme ve işlem hattını yeniden çalıştırma bağlantılarını içerir.

    ![İşlem hattı çalıştırmalarını izleme](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-pipeline-runs.png)

15. İşlem hattı çalıştırması ile ilişkili etkinlik çalıştırmalarını görüntülemek için seçin **etkinlik çalıştırmalarını görüntüle** bağlantısını **eylemleri** sütun. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. İşlem hattı çalıştırmaları görünümüne dönmek için seçin **işlem hatları** üstündeki bağlantısı. Listeyi yenilemek için **Yenile**’yi seçin. 

    ![Etkinlik çalıştırmalarını izleme](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-runs.png)

16. Her bir kopyalama etkinliği için yürütme ayrıntıları izlemek için **ayrıntıları** bağlantısını (gözlük resmi) altında **eylemleri** izleme görünümü etkinlik. Veri kaynağından kopyalanan havuz, veri aktarım hızı, yürütme adımları karşılık gelen süre ve yapılandırmaları kullanılan gibi ayrıntıları izleyebilirsiniz.

    ![İzleyici etkinlik çalışma ayrıntıları](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-run-details.png)

17. Verileri Azure Data Lake depolama Gen2 hesabınızda kopyalandığını doğrulayın.

## <a name="best-practices"></a>En iyi uygulamalar

Azure Data Lake depolama Gen1 genel olarak Azure Data Lake depolama Gen2 yükseltme değerlendirmek için bkz: [büyük veri analiz çözümlerinizi Azure Data Lake depolama 2. nesil için Azure Data Lake depolama Gen1 ' yükseltme](../storage/blobs/data-lake-storage-upgrade.md). Aşağıdaki bölümlerde, Data Lake depolama Gen1 Data Lake depolama Gen2'ye bir veri yükseltme için Data Factory kullanmaya yönelik en iyi yöntemler sunar.

### <a name="data-partition-for-historical-data-copy"></a>Veri bölümü için geçmiş veri kopyalama

- Data Lake depolama Gen1, toplam veri boyutu 30 TB'den az olduğunu ve dosya sayısını 1 milyondan az ise, tüm verilerini tek bir kopyalama etkinliği çalıştırma kopyalayabilirsiniz.
- Çok büyük miktarda veri kopyalamak için sahip olduğunuz veya toplu veri geçişi yönetme ve bunların her biri belirli bir zaman çerçevesi içindeki tüm yapmak için esneklik istiyorsanız, veri bölümleme. Bölümleme, beklenmeyen bir sorunla riskini azaltır.

Bir kavram kanıtı, uçtan uca çözüm doğrulayın ve kopyalama aktarım hızı, ortamınızda test etmek için kullanın. Önemli kavram kanıtı adımlar: 

1. Birkaç TB'a veri kopyalama performans taban çizgisi almak için Data Lake depolama Gen2 için Data Lake depolama Gen1 kopyalamak için tek bir kopyalama etkinliği ile bir Data Factory işlem hattı oluşturun. İle başlayan [veri tümleştirme birimleri (DIUs)](copy-activity-performance.md#data-integration-units) 128 olarak. 
2. 1\. adımda aldığınız kopyalama işleme bağlı olarak, tüm veri geçişi için gereken tahmini süre hesaplayın. 
3. (İsteğe bağlı) Bir denetim tablo oluşturun ve dosyaları geçirilmesi bölümlemek için dosya filtresi tanımlayın. Dosyalar bölümlemek için yolu şudur: 

    - Klasör adı veya klasör adı ile bir joker karakter filtresi bölümü. Bu yöntem öneririz.
    - Bölüm bir dosya tarafından son değiştirme zamanı.

### <a name="network-bandwidth-and-storage-io"></a>Ağ bant genişliği ve depolama g/ç 

Data Lake depolama Gen1 veri okuyan ve Data Lake depolama 2. nesil için veri yazma Data Factory kopyalama işi eşzamanlılık denetleyebilirsiniz. Bu şekilde, o depolama g/ç Data Lake depolama Gen1 normal iş çalışma geçiş işlemi sırasında etkilenmemesi için kullanım yönetebilirsiniz.

### <a name="permissions"></a>İzinler 

Data factory'de [Data Lake depolama Gen1 bağlayıcı](connector-azure-data-lake-store.md) hizmet sorumlusu ve yönetilen kimlik Azure kaynak kimlik doğrulama için destekler. [Data Lake depolama Gen2 bağlayıcı](connector-azure-data-lake-storage.md) hesap anahtarı, hizmet sorumlusu ve Azure kaynak kimlik doğrulama için yönetilen kimlik destekler. Data Factory gidin ve tüm dosyaları kopyalayın ya da yüksek erişim, okuma, veya tüm dosyaları yazma ve isterseniz ACL'leri ayarlamak için sağladığınız hesap için yeterli izinleri vermeniz denetim listeleri (ACL) erişimi olmasını sağlamak için. Bu, geçiş dönemi boyunca bir süper kullanıcı veya sahip rolü atayın. 

### <a name="preserve-acls-from-data-lake-storage-gen1"></a>Data Lake depolama Gen1 ACL'sinden koru

Data Lake depolama 2. nesil için Data Lake depolama Gen1 ' yükseltme yaptığınızda, veri dosyalarının yanı sıra ACL'leri çoğaltmak istiyorsanız, bkz. [Data Lake depolama Gen1 korumak ACL'sinden](connector-azure-data-lake-storage.md#preserve-acls-from-data-lake-storage-gen1). 

### <a name="incremental-copy"></a>Artımlı kopyalama 

Data Lake depolama Gen1 yalnızca yeni veya güncelleştirilmiş dosyaları yüklemek için çeşitli yaklaşımlar kullanabilirsiniz:

- Yeni veya güncelleştirilmiş dosyaları zaman bölümlenmiş klasör veya dosya adı tarafından yükleyin. Örnektir/2019/05/13 / *.
- Yeni veya güncelleştirilmiş dosyaları LastModifiedDate tarafından yükleyin.
- Herhangi bir üçüncü taraf aracı veya çözümü göre yeni veya güncelleştirilmiş dosyaları tanımlayın. Ardından dosya veya klasör adı için Data Factory işlem hattı parametresi veya bir tablo veya dosya aracılığıyla geçirin. 

Artımlı yükleme yapmak için uygun sıklığı Azure Data Lake depolama Gen1 dosyalarında toplam sayısı ve her seferinde yüklenecek yeni veya güncelleştirilmiş dosyaları hacmine bağlıdır. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
> [Azure Data Lake depolama Gen1 bağlayıcı](connector-azure-data-lake-store.md)
> [Azure Data Lake depolama Gen2 Bağlayıcısı](connector-azure-data-lake-storage.md)