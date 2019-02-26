---
title: "Hızlı Başlangıç: Azure Veri Gezgini ile Azure BLOB'ları alma"
description: Bu hızlı başlangıçta, Azure veri Gezgini'ne Event Grid aboneliği kullanarak depolama hesabı veri göndermek nasıl öğreneceksiniz.
services: data-explorer
author: radennis
ms.author: radennis
ms.reviewer: orspod
ms.service: data-explorer
ms.topic: quickstart
ms.date: 1/30/2019
ms.openlocfilehash: 6dac6fb18f221ddb45e5b5b7e325868915732368
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2019
ms.locfileid: "56804662"
---
# <a name="quickstart-ingest-azure-blobs-into-azure-data-explorer-by-subscribing-to-event-grid-notifications"></a>Hızlı Başlangıç: Event Grid Bildirimlere abone olarak, Azure veri Gezgini'ne Azure BLOB'ları alma

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve yüksek oranda ölçeklenebilir veri keşfetme hizmetidir. Azure Veri Gezgini, blob kapsayıcıları yazılan BLOB'ları (veriler yükleniyor) sürekli alma olanağı sağlar. Bu ayarı gerçekleştirilir bir [Azure Event Grid](/azure/event-grid/overview) abonelik için blob oluşturma olayları ve bu olayları bir Event hub'ı aracılığıyla Kusto yönlendirme. Bu hızlı başlangıçta, olay Hub'ına kendi bildirimleri gönderen bir Event Grid aboneliği olan bir depolama hesabı olmalıdır. Bir Event Grid veri bağlantısı oluşturma ve verileri Sistem genelindeki akış.

## <a name="prerequisites"></a>Önkoşullar

1. Azure aboneliğiniz yoksa, oluşturun bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/)
1. [Bir küme ve veritabanı](create-cluster-database-portal.md)
1. [Bir depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)
1. [Bir olay hub'ı](https://docs.microsoft.com/azure/event-hubs/event-hubs-create)

## <a name="create-an-event-grid-subscription-in-your-storage-account"></a>Depolama hesabınızdaki bir Event Grid aboneliği oluşturun

1. Azure portalında depolama hesabınıza gidin
1. Tıklayarak **olayları** sekmesini, sonra da **olay aboneliği**

    ![Sorgu uygulama bağlantısı](media/ingest-data-event-grid/create-event-grid-subscription.png)

1. İçinde **olay aboneliği oluşturma** pencereye **temel** sekmesinde, aşağıdaki değerleri sağlayın:

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Ad | *Test-grid-bağlantı* | Oluşturmak istediğiniz olay Kılavuzu adı.|
    | Olay Şeması | *Olay ızgarası şeması* | Event Grid için kullanılan şema. |
    | Konu Başlığı Türü | *Depolama hesabı* | Olay Kılavuzu konu başlığı türü. |
    | Konu Kaynağı | *gridteststorage* | Depolama hesabınızın adı. |
    | Tüm olay türlerine abone ol | *Seçeneğinin işaretini kaldırın* | Tüm olaylar hakkında size bir bildirim yapılmaz. |
    | Tanımlanan Olay Türleri | *Oluşturulan blob* | İçin bildirim almak için hangi belirli olayları. |
    | Uç Nokta Türü | *Event Hubs* | Olayları göndermek uç noktası türü. |
    | Uç Nokta | *test-hub* | Oluşturduğunuz olay hub'ı. |
    | | |

1. Seçin **ek özellikler** belirli bir kapsayıcıdan dosyaları izlemek istiyorsanız sekmesi. Bildirimler için filtreleri aşağıdaki gibi ayarlayın:
    * **Konu ile başlar** alandır *değişmez değer* önek blob kapsayıcısının (uygulanan deseni olarak *startswith*, birden çok kapsayıcı yayılabilir). Hiçbir joker karakterlere izin verilir.
     Bunu *gerekir* ayarlanması şu şekilde: *`/blobServices/default/containers/`*[kapsayıcı ön ek]
    * **Konu sona erer ile** alandır *değişmez değer* BLOB soneki. Hiçbir joker karakterlere izin verilir.

## <a name="create-a-target-table-in-azure-data-explorer"></a>Azure Veri Gezgini'nde hedef tablo oluşturma

Tablo, Event Hubs veri gönderip Azure veri Gezgini'nde oluşturun. Tablo kümeyi oluşturmak ve veritabanı adımında hazırlanan **önkoşulları**.

1. Azure portalda kümenizin altında **Sorgu**'yu seçin.

    ![Sorgu uygulama bağlantısı](media/ingest-data-event-grid/query-explorer-link.png)

1. Pencere ı seçin aşağıdaki komutu kopyalayın **çalıştırma** alınan verileri alır (TestTable) tablo oluşturun.

    ```Kusto
    .create table TestTable (TimeStamp: datetime, Value: string, Source:string)
    ```

    ![Oluşturma sorgusunu çalıştırma](media/ingest-data-event-grid/run-create-table.png)

1. Pencere ı seçin aşağıdaki komutu kopyalayın **çalıştırma** gelen JSON verileri sütun adları ve veri türleri tablosu (TestTable) için eşleme.

    ```Kusto
    .create table TestTable ingestion json mapping 'TestMapping' '[{"column":"TimeStamp","path":"$.TimeStamp"},{"column":"Value","path":"$.Value"},{"column":"Source","path":"$.Source"}]'
    ```

## <a name="create-an-event-grid-data-connection-in-azure-data-explorer"></a>Azure veri Gezgini'nde bir Event Grid veri bağlantısı oluşturma

Artık test tabloya blob kapsayıcısına akan veriler akışla böylece Event Grid için Azure veri Gezgini'nde bağlanın.

1. Olay hub'ı dağıtımının başarılı olduğundan emin olmak için araç çubuğunda **Bildirimler**'i seçin.

1. Oluşturduğunuz kümenin altında **Veritabanları**'nı ve ardından **TestDatabase** girişini seçin.

    ![Test veritabanını seçme](media/ingest-data-event-grid/select-test-database.png)

1. **Veri alımı**'nı ve ardından **Veri bağlantısı ekle**'yi seçin.

    ![Veri alımı](media/ingest-data-event-grid/data-ingestion-create.png)

1. Bağlantı türünü seçin: **BLOB Depolama**.

1. Formu aşağıdaki bilgilerle doldurun ve ardından tıklayın **Oluştur**.

    ![Olay hub'ı bağlantısı](media/ingest-data-event-grid/create-event-grid-data-connection.png)

     Veri kaynağı:

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Veri bağlantısı adı | *test-hub-connection* | Azure Veri Gezgini'nde oluşturmak istediğiniz bağlantının adı.|
    | Depolama hesabı aboneliği | Abonelik Kimliğiniz | Depolama hesabınızın bulunduğu abonelik kimliği.|
    | Depolama hesabı | *gridteststorage* | Daha önce oluşturduğunuz depolama hesabının adıdır.|
    | Event Grid | *Test-grid-bağlantı* | Oluşturduğunuz Event Grid adı. |
    | Olay Hub'ı adı | *test-hub* | Oluşturduğunuz olay hub'ı. Bir Event Grid seçtiğinizde bu otomatik olarak doldurulur. |
    | Tüketici grubu | *test-group* | Oluşturduğunuz olay hub'ında tanımlanan tüketici grubu. |
    | | |

    Hedef Tablo:

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Tablo | *TestTable* | **TestDatabase** içinde oluşturduğunuz tablo. |
    | Veri biçimi | *JSON* | Avro, CSV, JSON, çok SATIRLI JSON, PSV, SOH, SCSV, TSV ve TXT desteklenen biçimler:. |
    | Sütun eşleme | *TestMapping* | **TestDatabase** içinde oluşturduğunuz ve gelen JSON verilerini **TestTable** tablosunun sütun adları ve veri türleriyle eşleyen eşleme.|
    | | |

## <a name="generate-sample-data"></a>Örnek veri oluşturma

Azure Veri Gezgini ile depolama hesabı bağlı, örnek veri oluşturma ve blob depolama alanına yükleyin.

Azure Storage kaynakları ile etkileşim kurmak için birkaç temel Azure CLI komutlarını veren küçük bir kabuk betiği ile çalışırsınız. Betik ilk depolama hesabınızda yeni bir kapsayıcı oluşturur, ardından (blob) olarak mevcut bir dosyayı kapsayıcıya yükler. Sonra blobları kapsayıcıda listeler. Kullanabileceğiniz [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) doğrudan portalda betiği yürütülemedi.

Aşağıdaki veriler bir dosyaya kaydedin ve aşağıdaki komut dosyasını kullanın:

```Json
{"TimeStamp": "1987-11-16 12:00","Value": "Hello World","Source": "TestSource"}
```

```bash
#!/bin/bash
### A simple Azure Storage example script

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export file_to_upload=<file_to_upload>
    export destination_file=<destination_file>

    echo "Creating the container..."
    az storage container create --name $container_name

    echo "Uploading the file..."
    az storage blob upload --container-name $container_name --file $file_to_upload --name $blob_name

    echo "Listing the blobs..."
    az storage blob list --container-name $container_name --output table

    echo "Done"
```

## <a name="review-the-data-flow"></a>Veri akışını inceleme

> [!NOTE]
> ADX alma işlemi optimize etmek için tasarlanan veri alımı için bir toplama (toplu) ilkesi vardır.
Varsayılan olarak, ilkeyi 5 dakika ile yapılandırılır.
Gerektiğinde daha sonra ilkeyi değiştirmek mümkün olacaktır. Bu hızlı başlangıçta bir gecikme birkaç dakika sürmesini bekleyebilirsiniz.

1. Uygulama çalışırken Azure Portalı'nda, event grid altında ani etkinlik görürsünüz.

    ![Event grid grafiği](media/ingest-data-event-grid/event-grid-graph.png)

1. Veritabanına ulaşan ileti sayısını denetlemek için test veritabanınızda aşağıdaki sorguyu çalıştırın.

    ```Kusto
    TestTable
    | count
    ```

1. İleti içeriği görmek için test veritabanında aşağıdaki sorguyu çalıştırın.

    ```Kusto
    TestTable
    ```

    Sonuç kümesi aşağıdakine benzer olmalıdır.

    ![İleti sonuç kümesi](media/ingest-data-event-grid/table-result.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Yeniden, event grid'i kullanmayı planlamıyorsanız, temizleme **test-hub-rg**yinelenen maliyetler oluşmasını önlemek için.

1. Azure portalında, en solda bulunan **Kaynak grupları**’nı ve ardından oluşturduğunuz kaynak grubunu seçin.  

    Soldaki menü daraltılmışsa, genişletmek için ![Genişletme düğmesi](media/ingest-data-event-grid/expand.png) öğesine tıklayın.

   ![Silinecek kaynak grubunu seçin](media/ingest-data-event-grid/delete-resources-select.png)

1. **test-resource-group** altında **Kaynak grubunu sil**'i seçin.

1. Yeni pencerede silinecek kaynak grubunun adını yazın (*test-hub-rg*) ve **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure veri Gezgini'nde verileri Sorgulama](web-query-data.md)
