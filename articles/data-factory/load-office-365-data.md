---
title: Veri yükleme Office 365'ten Azure Data Factory kullanarak | Microsoft Docs
description: Office 365'ten veri kopyalamak için Azure Data factory'yi kullanın.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: jingwang
ms.openlocfilehash: fe3a3b673f6512856f3640b3e103db8623570a88
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445790"
---
# <a name="load-data-from-office-365-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Office 365'ten veri yükleme

Bu makalede Data factory'yi gösterilmektedir _veri yükleme Office 365'ten Azure Blob depolama alanına_. Azure Data Lake Gen1 veya 2. nesil veri kopyalamak için benzer adımları izleyebilirsiniz. Başvurmak [Office 365 Bağlayıcısı makalesi](connector-office-365.md) verileri Office 365'ten genel kopyalama.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüden **kaynak Oluştur** > **veri ve analiz** > **Data Factory**: 
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. İçinde **yeni veri fabrikası** sayfasında, aşağıdaki görüntüde gösterilen alanlar için değerleri sağlayın:
      
   ![Yeni veri fabrikası sayfası](./media/load-office-365-data/new-azure-data-factory.png)
 
    * **Ad**: Azure data factory'nizi için genel olarak benzersiz bir ad girin. Hatasını alırsanız "veri fabrikası adı \"LoadFromOffice365Demo\" kullanılabilir değil," veri fabrikası için farklı bir ad girin. Örneğin, adı kullanabilirsiniz  _**adınız**_**LoadFromOffice365Demo**. Veri Fabrikası oluşturmayı yeniden deneyin. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: Veri fabrikasının oluşturulacağı Azure aboneliğini seçin. 
    * **Kaynak grubu**: Aşağı açılan listeden mevcut bir kaynak grubunu seçin ya da seçin **Yeni Oluştur** seçenek ve bir kaynak grubu adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
    * **Sürüm**: Seçin **V2**.
    * **Konum**: Veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları başka konumlarda ve bölgelerde olabilir. Bu veri depolarına, Azure Data Lake Store, Azure depolama, Azure SQL veritabanı vb. içerir.

3. **Oluştur**’u seçin.
4. Oluşturma işlemi tamamlandıktan sonra veri fabrikanıza gidin. Gördüğünüz **Data Factory** aşağıdaki görüntüde gösterildiği gibi bir giriş sayfası:
   
   ![Data factory giriş sayfası](./media/load-office-365-data/data-factory-home-page.png)

5. Seçin **yazar ve İzleyici** veri tümleştirme uygulaması ayrı bir sekmede başlatmak için.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. "Başlayalım" sayfasında seçin **işlem hattı Oluştur**.
 
    ![İşlem hattı oluşturma](./media/load-office-365-data/create-pipeline-entry.png)

2. İçinde **Genel sekmesinde** "CopyPipeline" işlem hattı için girin **adı** işlem hattının.

3. Etkinlikler araç kutusundaki > taşıma ve dönüştürme Kategori > sürükle ve bırak **kopyalama etkinliği** işlem hattı tasarımcısının yüzeyine araç kutusundan. "CopyFromOffice365ToBlob" etkinlik adı belirtin.

### <a name="configure-source"></a>Kaynağı yapılandırma

1. İşlem hattı gidin > **kaynağı sekmesinde**, tıklayın **+ yeni** kaynak veri kümesi oluşturmak için. 

2. Yeni veri kümesi penceresinde **Office 365**ve ardından **son**.

    ![Yeni Office 365 veri kümesi](./media/load-office-365-data/new-office-365-dataset.png)
 
3. Yeni bir sekme açıldığını için Office 365 veri kümesi görürsünüz. Üzerinde **Genel sekmesinde** Özellikler penceresinin alt kısmında "SourceOffice365Dataset" adını girin.

    ![Genel yapılandırma Office 365 veri kümesi](./media/load-office-365-data/config-office-365-dataset-general.png)
 
4. Git **bağlantı sekmesi** Özellikler penceresinin. Bağlı hizmet metin kutusunun yanındaki tıklatın **+ yeni**.
 
    ![Office 365 config veri kümesi bağlantısı](./media/load-office-365-data/config-office-365-dataset-connection.png)

5. Yeni bağlı hizmet penceresi "Office365LinkedService" bir ad girin, hizmet sorumlusu kimliği ve hizmet sorumlusu anahtarı girin ve ardından bağlı hizmeti dağıtmak için Kaydet'i seçin.

    ![Yeni Office 365 bağlı hizmeti](./media/load-office-365-data/new-office-365-linked-service.png)
 
6. Bağlı hizmet oluşturulduktan sonra veri kümesi ayarlarına dönersiniz. Yanındaki "Tablo" mevcut Office 365 veri kümeleri listesini genişletmek için aşağı oku seçin ve "BasicDataSet_v0. Contact_v0 "aşağı açılan listeden:

    ![Office 365 config veri kümesi tablosu](./media/load-office-365-data/config-office-365-dataset-table.png)
 
7. Git **şema sekmesi** seçin ve Özellikler penceresinde **Şemayı içeri aktar**.  Kişi veri kümesi şema ve örnek değerleri görüntülendiğine dikkat edin.

    ![Office 365 config veri kümesi şema](./media/load-office-365-data/config-office-365-dataset-schema.png)

8. Şimdi, geri dönün işlem hattının > kaynak sekmesi, SourceBlobDataset'ın seçili olduğunu doğrulayın.
 
### <a name="configure-sink"></a>Havuzu yapılandırma

1. İşlem hattı gidin > **havuz sekmesini**seçip **+ yeni** havuz veri kümesi oluşturmak için.
 
2. Yeni veri kümesi penceresinde, yalnızca desteklenen hedef seçilirse, Office 365'ten kopyalarken dikkat edin. Seçin **Azure Blob Depolama**ve ardından **son**.  Bu öğreticide, Office 365 verileri bir Azure Blob depolama alanına kopyalayın.

    ![Yeni Blob veri kümesi](./media/load-office-365-data/new-blob-dataset.png)

4. Üzerinde **Genel sekmesinde** Özellikler penceresinin "OutputBlobDataset" adı girin.

5. Git **bağlantı sekmesi** Özellikler penceresinin. Bağlı hizmet metin kutusunu yanındaki seçme **+ yeni**.

    ![Yapılandırma blob'u veri kümesi bağlantısı](./media/load-office-365-data/config-blob-dataset-connection.png) 

6. Yeni bağlı hizmet penceresinde "AzureStorageLinkedService" bir ad girin, "hizmet sorumlusu" aşağı açılan listeden, kimlik doğrulama yöntemlerini seçin, hizmet uç noktası Kiracı hizmet sorumlusu kimliği, doldurun ve Kaydet'i seçin sonra birincil anahtar, hizmet bağlı hizmet dağıtın.  Başvuru [burada](connector-azure-blob-storage.md#service-principal-authentication) nasıl Azure Blob Depolama için hizmet sorumlusu kimlik doğrulaması ayarlamak için.

    ![Yeni Blob bağlı hizmeti](./media/load-office-365-data/new-blob-linked-service.png)

7. Bağlı hizmet oluşturulduktan sonra veri kümesi ayarlarına dönersiniz. Dosya yolu yanındaki seçin **Gözat** Office 365 verilerine ayıkladığınız için çıkış klasörünü seçin.  "Dosya biçimi ayarları", dosya biçimine yanındaki altında seçin "**JSON biçimine**" ve dosya deseni yanındaki "**nesne kümesini**".

    ![Yapılandırma blob'u veri kümesi yolu ve biçimi](./media/load-office-365-data/config-blob-dataset-path-and-format.png) 

8. İşlem hattı geri gidin > havuz sekmesini, OutputBlobDataset'ın seçili olduğunu doğrulayın.

## <a name="validate-the-pipeline"></a>İşlem hattını doğrulama

İşlem hattını doğrulamak için araç çubuğundan **Doğrula**'yı seçin.

Ayrıca sağ üst köşede kod tıklayarak işlem hattı çalıştırmasıyla ilişkili JSON kodunu da görebilirsiniz.

## <a name="publish-the-pipeline"></a>Yayımlama kanalı

Üst araç çubuğunda, seçin **tümünü Yayımla**. Bu eylem, oluşturduğunuz varlıkları (veri kümeleri ve işlem hatları) Data Factory'de yayımlar.

![Değişiklikleri yayımla](./media/load-office-365-data/publish-changes.png) 

## <a name="trigger-the-pipeline-manually"></a>İşlem hattını el ile tetikleme

Araç çubuğunda **Tetikleyici**’yi ve sonra **Şimdi Tetikle**’yi seçin. İşlem hattı çalıştırma sayfasında seçin **son**. 

## <a name="monitor-the-pipeline"></a>İşlem hattını izleme

Git **İzleyici sekmesi** soldaki. El ile tetikleme tarafından tetiklenmiş bir işlem hattı çalıştırması görürsünüz. Bağlantıları kullanabilirsiniz **Eylemler sütunundaki** etkinlik ayrıntılarını görüntülemek ve işlem hattını yeniden çalıştırın.

![İşlem hattını izleme](./media/load-office-365-data/pipeline-monitoring.png) 

İşlem hattı çalıştırması ile ilişkili etkinlik çalıştırmalarını görmek için seçin **etkinlik çalıştırmalarını görüntüle** Eylemler sütunundaki bağlantı. Bu örnekte, tek bir etkinlik olduğundan listede tek giriş görürsünüz. Kopyalama işlemiyle ilgili daha fazla bilgi için seçin **ayrıntıları bağlantısını (gözlük simgesi)** Eylemler sütunundaki.

![Etkinliğini izleme](./media/load-office-365-data/activity-monitoring.png) 

Bu ilk kez kullanıyorsanız, bu bağlam için verileri isteyen (veri erişimi tablo olmasından, hangi hedef hesap, içine yüklenen verilerdir ve hangi kullanıcı kimliğini veri yapıyor birlikte erişim isteği), kopyalama etkinliği görürsünüz. Durum olarak "**sürüyor**", yalnızca öğesini Eylemler altında "Details" bağlantısına tıkladığınızda, durum olarak görürsünüz "**RequesetingConsent**".  Veri erişim onaylayan grubunun bir üyesi, Privileged Access Management istekte veri ayıklama devam etmeden önce onaylamanız gerekir.

_İstekte bulunan onay olarak durumu:_
![Etkinlik yürütme ayrıntıları - istek onayı](./media/load-office-365-data/activity-details-request-consent.png) 

_Veri çıkarma olarak durumu:_

![Etkinlik yürütme ayrıntıları - veri ayıklamak](./media/load-office-365-data/activity-details-extract-data.png) 

Veri ayıklama devam edecek sağlanan onay verildikten sonra ve bir süre sonra işlem hattı çalıştırması tamamlandı olarak gösterilir.

![İzleyici ardışık düzen - başarılı](./media/load-office-365-data/pipeline-monitoring-succeeded.png) 

Artık Azure Blob Depolama alanı hedefine gidin ve Office 365 verilerini JSON biçiminde ayıklanan olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure SQL veri ambarı desteği hakkında bilgi edinmek için şu makaleye geçin: 

> [!div class="nextstepaction"]
>[Office 365 Bağlayıcısı](connector-office-365.md)