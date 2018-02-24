---
title: "Azure Data Lake Store için tanılama günlükleri görüntüleme | Microsoft Docs"
description: "Kurulum ve Azure Data Lake Store için tanılama günlüklerine erişmek öğrenme "
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: f6e75eb1-d0ae-47cf-bdb8-06684b7c0a94
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: b58a4b215b13d2e57a69a94a60e3e37471c926c8
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Azure Data Lake Store için tanılama günlüklerine erişme
Data Lake Store hesabınızı ve hesabınız için toplanan günlükleri görüntülemek nasıl günlüğü tanılama etkinleştirmek bilgi edinin.

Kuruluşlar, verileri ne sıklıkta erişildiğine, verilerine erişen kullanıcılara listesi gibi bilgiler sağlayan veri erişimi denetim izleri toplamak Azure Data Lake Store hesapları için tanılama günlük kaydını etkinleştirebilir ne kadar verinin depolanma biçimini hesap, vs. Etkinleştirildiğinde, tanılama ve/veya istekleri en iyi çaba ilkesine göre günlüğe kaydedilir. Hizmet uç noktası karşı yapılan istekleri varsa istekleri ve tanılama günlük girişleri oluşturulur.

## <a name="prerequisites"></a>Önkoşullar
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Store hesabı**. [Azure Portal'ı kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md) bölümündeki yönergeleri uygulayın.

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Data Lake Store hesabınız için tanılama günlük kaydını etkinleştir
1. Yeni [Azure Portal](https://portal.azure.com)'da oturum açın.
2. Data Lake Store hesabınızı açın ve Data Lake Store hesabı dikey penceresinden tıklayın **ayarları**ve ardından **tanılama günlükleri**.
3. İçinde **tanılama günlükleri** dikey penceresinde tıklatın **tanılamayı açın**.

    ![Tanılama günlük kaydını etkinleştir](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "tanılama günlüklerini etkinleştirme")

3. İçinde **tanılama** dikey penceresinde tanılama günlük kaydını yapılandırmak için aşağıdaki değişiklikleri yapın.
   
    ![Tanılama günlük kaydını etkinleştir](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "tanılama günlüklerini etkinleştirme")
   
   * İçin **adı**, tanılama günlük yapılandırması için bir değer girin.
   * Depolama/verileri farklı şekillerde işlemi seçebilirsiniz.
     
        * Seçeneğini **bir depolama hesabı arşive** bir Azure depolama hesabı günlükleri depolamak için. Daha sonraki bir tarihte toplu işlenen verileri arşivlemek istiyorsanız bu seçeneği kullanın. Bu seçeneği belirlerseniz günlüklerine kaydetmek için bir Azure depolama hesabı sağlamanız gerekir.
        
        * Seçeneğini **bir olay hub'ına akış** Azure olay Hub'ına günlük veri akışı için. Büyük olasılıkla gerçek zamanda gelen günlüklerini çözümlemek için bir aşağı akış işleme ardışık varsa bu seçeneği kullanır. Bu seçeneği belirlerseniz, kullanmak istediğiniz Azure olay Hub ayrıntılarını sağlamanız gerekir.

        * Seçeneğini **için günlük analizi Gönder** oluşturulan günlük verileri çözümlemek için Azure günlük analizi hizmeti kullanmak için. Bu seçeneği belirlerseniz, gerçekleştirme Günlük çözümlemesi kullanacağınız için Operations Management Suite çalışma ayrıntıları sağlamanız gerekir. Bkz: [görünüm veya günlük analizi günlük arama ile toplanan verileri çözümlemek](../log-analytics/log-analytics-tutorial-viewdata.md) günlük analizi kullanımıyla ilgili ayrıntılar için.
     
   * Denetim günlükleri veya istek günlüklerini veya her ikisini de almak isteyip istemediğinizi belirtin.
   * Verilerin korunması gereken gün sayısını belirtin. Bekletme, yalnızca günlük verileri arşivlemek için Azure depolama hesabı kullanıyorsanız geçerlidir.
   * **Kaydet**’e tıklayın.

Tanılama ayarları etkinleştirildiğinde, günlüklerde izleyebilirsiniz **tanılama günlüklerini** sekmesi.

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>Data Lake Store hesabınız için tanılama günlüklerini görüntüle
Data Lake Store hesabınız için günlük verileri görüntülemek için iki yolu vardır.

* Data Lake Store hesabından ayarlarını görüntüleme
* Verilerin depolandığı Azure depolama hesabından

### <a name="using-the-data-lake-store-settings-view"></a>Veri Gölü deposu ayarları görünümünü kullanma
1. Data Lake Store hesabınızdaki **ayarları** dikey penceresinde tıklatın **tanılama günlükleri**.
   
    ![Görünüm tanılama günlük](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "tanılama günlükleri görüntüleme") 
2. İçinde **tanılama günlüklerini** dikey penceresinde tarafından kategorilere günlükleri görmelisiniz **denetim günlüklerini** ve **isteği günlükleri**.
   
   * İstek günlüklerini Data Lake Store hesabında yapılan her API isteği yakalayın.
   * Denetim günlüklerini günlükleri isteği ancak Data Lake Store hesabı üzerinde gerçekleştirilen işlemler çok daha ayrıntılı bir dökümünü sağlamak için benzerdir. Örneğin, bir istek günlüklerini tek karşıya yükleme API çağrısında denetim günlüklerini "Ekle" işlemlerinde neden olabilir.
3. Günlükleri indirmek için tıklayın **karşıdan** her günlük girişinin karşı bağlantı.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Azure depolama hesabından günlük verilerini içeren
1. Günlüğe kaydetme için Data Lake Store ile ilişkili Azure depolama hesabı dikey penceresini açın ve Bloblar'a tıklayın. **Blob hizmeti** iki kapsayıcı dikey penceresinde listelenir.
   
    ![Görünüm tanılama günlük](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "tanılama günlükleri görüntüleme")
   
   * Kapsayıcı **Öngörüler günlükleri denetim** denetim günlüklerini içerir.
   * Kapsayıcı **Öngörüler günlükleri istekleri** isteği günlükleri içerir.
2. Bu kapsayıcılara günlükleri aşağıdaki yapısı altında depolanır.
   
    ![Görünüm tanılama günlük](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "tanılama günlükleri görüntüleme")
   
    Örnek olarak, Denetim günlüğü tam yolunu olabilir `https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`
   
    Benzer şekilde, bir istek günlüğü tam yolunu olabilir `https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>Günlük verilerinin yapısını anlama
Denetim ve istek günlüklerini bir JSON biçimindedir. Bu bölümde, JSON yapısı istek için bakın ve Denetim günlükleri.

### <a name="request-logs"></a>İstek günlükleri
Burada JSON biçimli istek günlüğüne örnek girişi verilmiştir. Her bir blob olarak adlandırılan bir kök nesnesi vardır **kayıtları** günlük nesnelerinin bir dizisi içerir.

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>İstek günlüğü şeması
| Ad | Tür | Açıklama |
| --- | --- | --- |
| time |Dize |Timestamp (UTC) günlük |
| resourceId |Dize |İşlemi sürdü kaynak Kimliğini yerleştirin |
| category |Dize |Günlük kategorisi. Örneğin, **istekleri**. |
| operationName |Dize |Oturum açmış işlemin adı. Örneğin, getfilestatus. |
| resultType |Dize |Örneğin, 200 işlem durumu. |
| callerIpAddress |Dize |İsteği yapan istemcinin IP adresi |
| correlationId |Dize |Bir dizi ilgili günlük girişlerini gruplamak için kullanılan kimliği için günlük |
| identity |Nesne |Günlük oluşturulan kimliği |
| properties |JSON |Ayrıntılar için aşağıya bakın |

#### <a name="request-log-properties-schema"></a>İstek günlüğü özellikleri şeması
| Ad | Tür | Açıklama |
| --- | --- | --- |
| HttpMethod |Dize |İşlem için kullanılan HTTP yöntem. Örneğin, alın. |
| Yol |Dize |Yolun işlemi üzerinde gerçekleştirildi |
| RequestContentLength |Int |HTTP isteğinin içerik uzunluğu |
| ClientRequestId |Dize |Bu istek benzersiz olarak tanıtan kimliği |
| StartTime |Dize |Sunucu isteği aldığınız zaman |
| EndTime |Dize |Sunucu yanıt gönderilme saati |

### <a name="audit-logs"></a>Denetim günlükleri
Burada JSON biçimli Denetim günlüğüne örnek girişi verilmiştir. Her bir blob olarak adlandırılan bir kök nesnesi vardır **kayıtları** günlük nesnelerinin bir dizisi içerir

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Denetim günlüğü şeması
| Ad | Tür | Açıklama |
| --- | --- | --- |
| time |Dize |Timestamp (UTC) günlük |
| resourceId |Dize |İşlemi sürdü kaynak Kimliğini yerleştirin |
| category |Dize |Günlük kategorisi. Örneğin, **denetim**. |
| operationName |Dize |Oturum açmış işlemin adı. Örneğin, getfilestatus. |
| resultType |Dize |Örneğin, 200 işlem durumu. |
| correlationId |Dize |Bir dizi ilgili günlük girişlerini gruplamak için kullanılan kimliği için günlük |
| identity |Nesne |Günlük oluşturulan kimliği |
| properties |JSON |Ayrıntılar için aşağıya bakın |

#### <a name="audit-log-properties-schema"></a>Denetim günlüğü özellikleri şeması
| Ad | Tür | Açıklama |
| --- | --- | --- |
| StreamName |Dize |Yolun işlemi üzerinde gerçekleştirildi |

## <a name="samples-to-process-the-log-data"></a>Günlük verileri işlemek için örnekleri
Günlükleri Azure günlük analizi için Azure Data Lake Deposu'ndan veri gönderirken (bkz [görünüm veya günlük analizi günlük arama ile toplanan verileri çözümlemek](../log-analytics/log-analytics-tutorial-viewdata.md) günlük analizi kullanımıyla ilgili ayrıntılar için), aşağıdaki sorguda kullanıcı listesini içeren bir tablo döndürür adları, bir görsel grafik birlikte olay süresi için zaman olayların ve etkinliklerin sayısını görüntüler. Kullanıcı GUID'si göstermek için kolayca değiştirilebilir veya diğer öznitelikleri:

```
search *
| where ( Type == "AzureDiagnostics" )
| summarize count(TimeGenerated) by identity_s, TimeGenerated
```


Azure Data Lake Store işlemek ve günlük verilerini analiz konusunda bir örnek sağlar. Örnek: bulabilirsiniz [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md)
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)

