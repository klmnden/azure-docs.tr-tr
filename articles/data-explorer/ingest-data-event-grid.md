---
title: Azure Veri Gezgini ile Azure BLOB'ları alma
description: Bu makalede, Azure veri Gezgini'ne Event Grid aboneliği kullanarak depolama hesabı veri gönderme konusunda bilgi edinin.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 7d9c21b46f760055846194f52f1594f25b1ee989
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66494740"
---
# <a name="ingest-blobs-into-azure-data-explorer-by-subscribing-to-event-grid-notifications"></a>Event Grid Bildirimlere abone olarak, Azure veri Gezgini'ne BLOB'ları alma

Azure Veri Gezgini, günlük ve telemetri verilerini için hızlı ve ölçeklenebilir bir veri araştırma hizmetidir. Bu, blob kapsayıcıları yazılan bloblarından sürekli alma (veriler yükleniyor) olanağı sağlar. 

Bu makalede, nasıl ayarlanacağını öğreneceksiniz bir [Azure Event Grid](/azure/event-grid/overview) aboneliği ve Azure Veri Gezgini rota olaylara bir event hub'ı aracılığıyla. Başlamak için Azure Event Hubs'a bildirimleri gönderen bir event grid aboneliği olan bir depolama hesabı olmalıdır. Bir Event Grid veri bağlantısı oluşturacak ve verileri sonra Sistem genelindeki akış.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Oluşturma bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/).
* [Bir küme ve veritabanı](create-cluster-database-portal.md).
* [Bir depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal).
* [Bir olay hub'ı](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

## <a name="create-an-event-grid-subscription-in-your-storage-account"></a>Depolama hesabınızdaki bir Event Grid aboneliği oluşturun

1. Azure portalında, depolama hesabınızı bulun.
1. Seçin **olayları** > **olay aboneliği**.

    ![Sorgu uygulama bağlantısı](media/ingest-data-event-grid/create-event-grid-subscription.png)

1. İçinde **olay aboneliği oluşturma** pencereye **temel** sekmesinde, aşağıdaki değerleri sağlayın:

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Ad | *Test-grid-bağlantı* | Oluşturmak istediğiniz olay Kılavuzu adı.|
    | Olay şeması | *Olay ızgarası şeması* | Event grid için kullanılan şema. |
    | Konu Türü | *Depolama hesabı* | Olay Kılavuzu konu başlığı türü. |
    | Konu kaynak | *gridteststorage* | Depolama hesabınızın adı. |
    | Tüm olay türlerine abone ol | *Temizle* | Tüm olaylar hakkında size bir bildirim yapılmaz. |
    | Tanımlı olay türleri | *Oluşturulan blob* | İçin bildirim almak için hangi belirli olayları. |
    | Uç noktası türü | *Olay hub'ları* | Olayları göndermek uç noktası türü. |
    | Uç Nokta | *test-hub* | Oluşturduğunuz olay hub'ı. |
    | | |

1. Seçin **ek özellikler** belirli bir kapsayıcıdan dosyaları izlemek istiyorsanız sekmesi. Bildirimler için filtreleri aşağıdaki gibi ayarlayın:
    * **Konu ile başlar** alandır *değişmez değer* blob kapsayıcısının öneki. Uygulanan deseni olarak *startswith*, birden çok kapsayıcı yayılabilir. Hiçbir joker karakterlere izin verilir.
     Bunu *gerekir* ayarlanması şu şekilde: *`/blobServices/default/containers/`* [kapsayıcı ön ek]
    * **Konu sona erer ile** alandır *değişmez değer* BLOB soneki. Hiçbir joker karakterlere izin verilir.

## <a name="create-a-target-table-in-azure-data-explorer"></a>Azure Veri Gezgini'nde hedef tablo oluşturma

Azure veri burada Event Hubs veri gönderip Gezgini'nde bir tablo oluşturun. Küme ve önkoşullarda hazırlanan bir veritabanı tablosu oluşturun.

1. Azure portalda kümenizin altında **Sorgu**'yu seçin.

    ![Sorgu uygulama bağlantısı](media/ingest-data-event-grid/query-explorer-link.png)

1. Aşağıdaki komutu seçin ve penceresi kopyalamak **çalıştırma** alınan verileri alır (TestTable) tablo oluşturmak için.

    ```Kusto
    .create table TestTable (TimeStamp: datetime, Value: string, Source:string)
    ```

    ![Oluşturma sorgusunu çalıştırma](media/ingest-data-event-grid/run-create-table.png)

1. Pencere ı seçin aşağıdaki komutu kopyalayın **çalıştırma** gelen JSON verileri sütun adları ve veri türleri tablosu (TestTable) için eşleme.

    ```Kusto
    .create table TestTable ingestion json mapping 'TestMapping' '[{"column":"TimeStamp","path":"$.TimeStamp"},{"column":"Value","path":"$.Value"},{"column":"Source","path":"$.Source"}]'
    ```

## <a name="create-an-event-grid-data-connection-in-azure-data-explorer"></a>Azure veri Gezgini'nde bir Event Grid veri bağlantısı oluşturma

Böylece test tabloya blob kapsayıcısına akan veriler akışla event grid için Azure veri Gezgini'nde bağlanın.

1. Olay hub'ı dağıtımının başarılı olduğundan emin olmak için araç çubuğunda **Bildirimler**'i seçin.

1. Oluşturduğunuz kümeyi altında seçin **veritabanları** > **TestDatabase**.

    ![Test veritabanını seçme](media/ingest-data-event-grid/select-test-database.png)

1. Seçin **veri alımı** > **veri bağlantısı ekleme**.

    ![Veri alımı](media/ingest-data-event-grid/data-ingestion-create.png)

1.  Bağlantı türünü seçin: **BLOB Depolama**.

1. Formu aşağıdaki bilgilerle doldurun ve seçin **Oluştur**.

    ![Olay hub'ı bağlantısı](media/ingest-data-event-grid/create-event-grid-data-connection.png)

     Veri kaynağı:

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Veri bağlantısı adı | *test-hub-connection* | Azure veri Gezgini'nde oluşturmak istediğiniz bağlantının adıdır.|
    | Depolama hesabı aboneliği | Abonelik Kimliğiniz | Depolama hesabınızın bulunduğu abonelik kimliği.|
    | Depolama hesabı | *gridteststorage* | Daha önce oluşturduğunuz depolama hesabının adı.|
    | Event Grid | *Test-grid-bağlantı* | Oluşturduğunuz event grid adı. |
    | Olay Hub'ı adı | *test-hub* | Oluşturduğunuz olay hub'ı. Bu alan, bir olay Kılavuzu seçtiğinizde otomatik olarak doldurulur. |
    | Tüketici grubu | *test-group* | Tüketici grubu olay hub'da oluşturduğunuz tanımlı. |
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

Azure Storage kaynakları ile etkileşim kurmak için birkaç temel Azure CLI komutlarını veren küçük bir kabuk betiği ile çalışırsınız. Bu betik, depolama hesabınızda yeni bir kapsayıcı oluşturur, mevcut bir dosyayı (bir blobu olarak), kapsayıcıya yükler ve ardından kapsayıcıdaki blobları listeler. Kullanabileceğiniz [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) doğrudan portalda betiği yürütülemedi.

Verileri bir dosyaya kaydedin ve bu betik ile yükleyin:

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
> Azure Veri Gezgini, bir toplama (toplu) ilke alma işlemi optimize etmek için tasarlanan veri alımı için vardır.
Varsayılan olarak, ilkeyi 5 dakika ile yapılandırılır.
Gerekirse daha sonra ilkeyi değiştirmek mümkün olacaktır. Bu makalede bir gecikme birkaç dakika sürmesini bekleyebilirsiniz.

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

1. Yeni pencerede, silmek için kaynak grubunun adını girin (*test-hub-rg*) ve ardından **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure veri Gezgini'nde verileri Sorgulama](web-query-data.md)
