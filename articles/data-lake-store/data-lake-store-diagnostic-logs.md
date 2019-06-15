---
title: Azure Data Lake depolama Gen1 için tanılama günlüklerini görüntüleme | Microsoft Docs
description: 'Kurulum ve tanılama günlüklerine erişmek için Azure Data Lake depolama Gen1 öğrenin '
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: f6e75eb1-d0ae-47cf-bdb8-06684b7c0a94
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: twooley
ms.openlocfilehash: d200f72b3c0e5634c3dca8f60a4754a14351110a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60878752"
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-storage-gen1"></a>Azure Data Lake depolama Gen1 için tanılama günlüklerine erişme
Azure Data Lake depolama Gen1 hesabınızı ve hesabınız için toplanan günlükleri görüntülemek için günlüğe kaydetme tanılama etkinleştirmeyi öğrenin.

Kuruluşlar, verilerin ne sıklıkla erişilen verileri erişen kullanıcıların listesi gibi bilgileri sağlayan veri erişimi denetim izleri toplamak Azure Data Lake depolama Gen1 hesabı tanılama günlük kaydını etkinleştirebilirsiniz ne kadar veri olarak depolanır Hesap, vs. Etkin olduğunda, bir en iyi çaba ilkesine göre tanılama ve/veya istekleri günlüğe kaydedilir. Hizmet uç noktasına karşı yapılan istekler varsa hem istekleri hem de tanılama günlük girişi oluşturulur.

## <a name="prerequisites"></a>Önkoşullar
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake depolama Gen1 hesabı**. Konumundaki yönergeleri [Azure Data Lake depolama Gen1 ile çalışmaya başlama Azure portalını kullanarak](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-storage-gen1-account"></a>Data Lake depolama Gen1 hesabınız için tanılama günlüğünü etkinleştirme
1. Yeni [Azure Portal](https://portal.azure.com)'da oturum açın.
2. Data Lake depolama Gen1 hesabınızı açın ve Data Lake depolama Gen1 hesabı dikey penceresinden tıklayın **tanılama ayarları**.
3. İçinde **tanılama ayarları** dikey penceresinde tıklayın **tanılamayı Aç**.

    ![Tanılama günlüğünü etkinleştirme](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "tanılama günlüklerini etkinleştirme")

3. İçinde **tanılama ayarları** dikey penceresinde, tanılama günlük kaydını yapılandırmak için aşağıdaki değişiklikleri yapın.
   
    ![Tanılama günlüğünü etkinleştirme](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "tanılama günlüklerini etkinleştirme")
   
   * İçin **adı**, tanılama günlüğü yapılandırması için bir değer girin.
   * Depolama/verilerin farklı yollarla işlemek seçebilirsiniz.
     
        * Seçeneğini **bir depolama hesabında arşivle** bir Azure depolama hesabına günlüklerini depolamak için. Daha sonraki bir tarihte batch işlenen verileri arşivlemek istiyorsanız bu seçeneği kullanın. Bu seçeneği belirlerseniz günlüklere kaydetmek için bir Azure depolama hesabı sağlamanız gerekir.
        
        * Seçeneğini **Stream olay hub'ına** Azure olay Hub'ına günlük veri akışı. Büyük olasılıkla gerçek zamanda gelen günlükleri analiz etmek için bir aşağı akış işleme işlem hattı varsa bu seçeneği kullanın. Bu seçeneği belirlerseniz, Azure Event Hub kullanmak istediğiniz için ayrıntıları sağlamanız gerekir.

        * Seçeneğini **Log Analytics'e gönderme** oluşturulan günlük verileri analiz etmek için Azure İzleyici hizmeti kullanmak için. Bu seçeneği belirlerseniz, günlük analizini gerçekleştirin kullanacağınız için Log Analytics çalışma alanı ayrıntılarını sağlamanız gerekir. Bkz: [görünümü veya Azure İzleyici günlük araması ile toplanan verileri analiz etmek](../azure-monitor/learn/tutorial-viewdata.md) Azure İzleyici kullanımıyla ilgili ayrıntılar için günlüğe kaydeder.
     
   * Denetim günlükleri veya istek günlükleri veya her ikisi de almak isteyip istemediğinizi belirtin.
   * Verilerin korunması gereken gün sayısını belirtin. Bekletme yalnızca günlük verileri arşivlemek için Azure depolama hesabı kullanıyorsanız geçerlidir.
   * **Kaydet**’e tıklayın.

Tanılama ayarları etkinleştirildikten sonra günlüklerde izleyebilirsiniz **tanılama günlükleri** sekmesi.

## <a name="view-diagnostic-logs-for-your-data-lake-storage-gen1-account"></a>Data Lake depolama Gen1 hesabınız için tanılama günlüklerini görüntüleme
Data Lake depolama Gen1 hesabınız için günlük verileri görüntülemek için iki yolu vardır.

* Data Lake depolama Gen1 hesap ayarları görüntüleyin
* Verilerin depolandığı Azure depolama hesabından

### <a name="using-the-data-lake-storage-gen1-settings-view"></a>Data Lake depolama Gen1 ayarlarını görünümünü kullanma
1. Data Lake depolama Gen1 hesabınızdan **ayarları** dikey penceresinde tıklayın **tanılama günlükleri**.
   
    ![Tanılama günlükleri görüntüleyebilir](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "tanılama günlüklerini görüntüleme") 
2. İçinde **tanılama günlükleri** dikey penceresinde tarafından kategorilere ayrılarak günlükleri görmeniz **denetim günlüklerini** ve **istek günlükleri**.
   
   * İstek günlükleri Data Lake depolama Gen1 hesabında yaptığınız her API isteği yakalayın.
   * Günlükleri iste ancak Data Lake depolama Gen1 hesapta gerçekleştirilmekte olan işlemlerin daha ayrıntılı bir dökümü sağlamak için denetim günlüklerini benzerdir. Örneğin, bir istek günlükleri tek bir karşıya yükleme API çağrısında birden çok "Ekleme" işlemleri denetim günlüklerinde neden olabilir.
3. Günlükleri indirmek için tıklayın **indirme** her günlük girişinin karşı bağlantı.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Günlük verilerini içeren Azure depolama hesabından
1. Günlüğe kaydetme için Data Lake depolama Gen1 ile ilişkili Azure depolama hesabı dikey penceresini açın ve ardından Bloblar'a tıklayın. **Blob hizmeti** iki kapsayıcı dikey penceresinde listelenir.
   
    ![Tanılama günlüğüne kaydetme görünümü](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "tanılama günlüklerini görüntüleme")
   
   * Kapsayıcı **ınsights günlükleri denetim** denetim günlükleri içeriyor.
   * Kapsayıcı **ınsights günlükleri istekleri** istek günlükleri içerir.
2. Bu kapsayıcılara günlükleri aşağıdaki yapısı altında depolanır.
   
    ![Tanılama günlüğüne kaydetme görünümü](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "tanılama günlüklerini görüntüleme")
   
    Örneğin, bir denetim günlüğüne tam yolu olabilir `https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestorage/y=2016/m=07/d=18/h=04/m=00/PT1H.json`
   
    Benzer şekilde, bir istek günlüğü tam yolu olabilir `https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestorage/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>Günlük veri yapısını anlayın
Denetim ve istek günlükleri bir JSON biçimindedir. Bu bölümde, istek için JSON yapısını bakmak ve Denetim günlükleri.

### <a name="request-logs"></a>Günlükleri iste
JSON biçimli istek günlüğünde örnek giriş aşağıdadır. Her blob olarak adlandırılan bir kök nesnesinin sahip **kayıtları** günlük nesnelerinin bir dizisi içeren.

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_storage_gen1_account_name>",
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
| time |String |Zaman damgası (UTC) günlüğü |
| resourceId |String |İşlem geçen kaynak Kimliğini yerleştireceğinize |
| category |String |Günlük kategorisi. Örneğin, **istekleri**. |
| operationName |String |Oturum açmış işlemin adı. Örneğin, getfilestatus. |
| resultType |String |Örneğin, 200 işlem durumu. |
| callerIpAddress |String |İsteği yapan istemcinin IP adresi |
| correlationId |String |Bir dizi ilgili günlük girişlerini gruplamak için kullanılan kimliği için günlük |
| identity |Object |Günlük oluşturulan kimlik |
| properties |JSON |Ayrıntılar için aşağıya bakın |

#### <a name="request-log-properties-schema"></a>İstek günlüğü özellikleri şeması
| Ad | Tür | Açıklama |
| --- | --- | --- |
| HttpMethod |String |HTTP yöntemi, bir işlem için kullanılmaz. Örneğin, alın. |
| `Path` |String |İşlem yolu üzerinde gerçekleştirildi |
| RequestContentLength |int |HTTP isteğinin içerik uzunluğu |
| Clientrequestıd'ye |String |Bu istek benzersiz olarak tanımlayan kimliği |
| StartTime |String |Sunucu isteği aldığınız zaman |
| EndTime |String |Sunucu yanıt gönderme zamanı |

### <a name="audit-logs"></a>Denetim günlükleri
JSON biçimli bir denetim günlüğüne örnek girişini İşte. Her blob olarak adlandırılan bir kök nesnesinin sahip **kayıtları** günlük nesnelerinin bir dizisi içeren

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_storage_gen1_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "resultSignature": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_storage_gen1_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Denetim günlüğü şeması
| Ad | Tür | Açıklama |
| --- | --- | --- |
| time |String |Zaman damgası (UTC) günlüğü |
| resourceId |String |İşlem geçen kaynak Kimliğini yerleştireceğinize |
| category |String |Günlük kategorisi. Örneğin, **denetim**. |
| operationName |String |Oturum açmış işlemin adı. Örneğin, getfilestatus. |
| resultType |String |Örneğin, 200 işlem durumu. |
| resultSignature |String |İşlemi hakkında daha fazla ayrıntı. |
| correlationId |String |Bir dizi ilgili günlük girişlerini gruplamak için kullanılan kimliği için günlük |
| identity |Object |Günlük oluşturulan kimlik |
| properties |JSON |Ayrıntılar için aşağıya bakın |

#### <a name="audit-log-properties-schema"></a>Denetim günlüğü özellikleri şeması
| Ad | Tür | Açıklama |
| --- | --- | --- |
| StreamName |String |İşlem yolu üzerinde gerçekleştirildi |

## <a name="samples-to-process-the-log-data"></a>Günlük verileri işlemek için örnekleri
Günlükleri, Azure Data Lake depolama Gen1 Azure İzleyici günlüklerine gönderirken, (bkz [görünümü veya Azure İzleyici günlük araması ile toplanan verileri analiz etmek](../azure-monitor/learn/tutorial-viewdata.md) Azure İzleyici kullanımıyla ilgili ayrıntılar için günlükleri), aşağıdaki sorguyu içeren bir tablo döndürür bir Kullanıcı listesi görünen adlar, olayların süresi ve olay sayısı için görsel bir grafiğin yanı sıra etkinliğin saati. Kullanıcı GUID'si göstermek için kolayca değiştirilebilir ya da diğer öznitelikleri:

```
search *
| where ( Type == "AzureDiagnostics" )
| summarize count(TimeGenerated) by identity_s, TimeGenerated
```


Azure Data Lake depolama Gen1 günlük verilerini işlemek ve çözümlemek nasıl bir örnek sağlar. Örneğini şurada bulabilirsiniz [ https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample ](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake depolama Gen1 genel bakış](data-lake-store-overview.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)

