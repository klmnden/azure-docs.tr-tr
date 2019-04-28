---
title: U-SQL betiği - Azure'ı kullanarak verileri dönüştürme | Microsoft Docs
description: Azure Data Lake Analytics işlem hizmetini U-SQL betikleri çalıştırarak işleme veya dönüştürme veri öğrenin.
services: data-factory
documentationcenter: ''
ms.assetid: e17c1255-62c2-4e2e-bb60-d25274903e80
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/01/2017
author: nabhishek
ms.author: abnarain
manager: craigg
robots: noindex
ms.openlocfilehash: 5835c37363c7e9d2dd3253c08ab97f17852725f5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61248156"
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Üzerinde Azure Data Lake Analytics U-SQL betikleri çalıştırarak verileri dönüştürme 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-usql-activity.md)
> * [Sürüm 2 (geçerli sürüm)](../transform-data-using-data-lake-analytics.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [U-SQL etkinliği V2'de](../transform-data-using-data-lake-analytics.md).

Bir Azure data factory'de bir işlem hattı, bağlantılı depolama Hizmetleri'ndeki veri bağlı işlem hizmetlerini kullanarak işler. Bu etkinliklerin nerede her etkinlik bir özel işleme işlemi gerçekleştirir dizisi içerir. Bu makalede **Data Lake Analytics U-SQL etkinliği** çalıştırılan bir **U-SQL** üzerinde komut bir **Azure Data Lake Analytics** işlem bağlı hizmeti. 

Bir Azure Data Lake Analytics hesabı bir Data Lake Analytics U-SQL etkinliği ile işlem hattı oluşturmadan önce oluşturun. Azure Data Lake Analytics hakkında bilgi edinmek için [Azure Data Lake Analytics ile çalışmaya başlama](../../data-lake-analytics/data-lake-analytics-get-started-portal.md).

Gözden geçirme [, ilk işlem hattı öğretici derleme](data-factory-build-your-first-pipeline.md) bir veri fabrikası oluşturmak ayrıntılı adımlar için Hizmetleri, veri kümeleri ve işlem hattı bağlı. JSON parçacığı, Data Factory Düzenleyici, Visual Studio veya Azure PowerShell ile Data Factory varlıkları oluşturmak için kullanın.

## <a name="supported-authentication-types"></a>Kimlik doğrulaması türleri desteklenir
U-SQL etkinliği, Data Lake Analytics karşı kimlik doğrulama türleri aşağıda destekler:
* Hizmet sorumlusu kimlik doğrulaması
* Kullanıcı kimlik bilgisi (OAuth) kimlik doğrulaması 

Özellikle bir zamanlanmış U-SQL yürütme için hizmet sorumlusu kimlik doğrulaması kullanmanızı öneririz. Belirteç sona erme davranış, kullanıcı kimlik bilgilerinin ile ortaya çıkabilir. Yapılandırma ayrıntıları için bkz. [bağlı hizmeti özellikleri](#azure-data-lake-analytics-linked-service) bölümü.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics bağlı hizmeti
Oluşturduğunuz bir **Azure Data Lake Analytics** bir Azure Data Lake Analytics bağlamak için bağlı hizmet bir Azure data factory hizmetine işlem. Data Lake Analytics U-SQL etkinliği işlem hattındaki bu bağlı hizmetini ifade eder. 

Aşağıdaki tabloda JSON tanımında kullanılan genel özellikleri için açıklamalar sağlar. Daha fazla hizmet sorumlusu ve kullanıcı kimlik bilgileri doğrulaması arasında seçim yapabilirsiniz.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **type** |Type özelliği ayarlanmalıdır: **AzureDataLakeAnalytics**. |Evet |
| **accountName** |Azure Data Lake Analytics hesap adı. |Evet |
| **dataLakeAnalyticsUri** |Azure Data Lake Analytics URI. |Hayır |
| **Subscriptionıd** |Azure abonelik kimliği |Hayır (belirtilmezse, data Factory abonelik kullanılır). |
| **resourceGroupName** |Azure kaynak grubu adı |Hayır (belirtilmezse, data Factory kaynak grubu kullanılır). |

### <a name="service-principal-authentication-recommended"></a>(Önerilen) hizmet sorumlusu kimlik doğrulaması
Hizmet sorumlusu kimlik doğrulaması kullanmak için Azure Active Directory (Azure AD) uygulama varlığı Kaydet ve Data Lake Store erişimi verin. Ayrıntılı adımlar için bkz. [hizmetten hizmete kimlik doğrulaması](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:
* Uygulama Kimliği
* Uygulama anahtarı 
* Kiracı Kimliği

Hizmet sorumlusu kimlik doğrulaması, aşağıdaki özellikleri belirterek kullanın:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **servicePrincipalId** | Uygulamanın istemci kimliği belirtin. | Evet |
| **serviceprincipalkey değerleri** | Uygulama anahtarını belirtin. | Evet |
| **Kiracı** | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. | Evet |

**Örnek: Hizmet sorumlusu kimlik doğrulaması**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Kullanıcı kimlik bilgileri doğrulaması
Alternatif olarak, aşağıdaki özellikleri belirterek Data Lake Analytics için kullanıcı kimlik bilgilerinin kullanabilirsiniz:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **Yetkilendirme** | Tıklayın **Authorize** düğmesini Data Factory Düzenleyicisi'nde ve bu özelliği otomatik olarak oluşturulan yetkilendirme URL'si atar kimlik bilgilerinizi girin. | Evet |
| **sessionId** | OAuth yetkilendirme oturumundan OAuth oturum kimliği. Her oturum kimliği benzersiz olup yalnızca bir kez kullanılabilir. Bu ayar, Data Factory Düzenleyicisi'ni kullandığınızda otomatik olarak oluşturulur. | Evet |

**Örnek: Kullanıcı kimlik bilgileri doğrulaması**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Belirteç süre sonu
Oluşturulan kullanarak Yetkilendirme kodu **Authorize** düğmesi süre sonra süresi dolar. Farklı türlerdeki kullanıcı hesapları için zaman aşımı süresinin için aşağıdaki tabloya bakın. Şu hatayı görebilirsiniz ne zaman ileti kimlik doğrulama **belirtecinin süresi dolmadan**: Kimlik bilgileri işlemi hatası: invalid_grant - AADSTS70002: Kimlik bilgileri doğrulanırken hata. AADSTS70008: Sağlanan erişim izni süresi doldu veya iptal edildi. İzleme kimliği: d18629e8-af88-43c5-88e3-d8419eb1fca1 bağıntı kimliği: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 zaman damgası: 2015-12-15 21:09:31Z

| Kullanıcı türü | Sürenin dolacağı tarih |
|:--- |:--- |
| Azure Active Directory tarafından yönetilmeyen kullanıcı hesapları (@hotmail.com, @live.comvb..) |12 saat |
| Azure Active Directory (AAD) yönetilen kullanıcı hesapları |14 gün sonra en son dilim çalıştırın. <br/><br/>90 OAuth tabanlı bağlı hizmetini temel alan bir dilimi 14 günde en az bir kez çalışıyorsa gün. |

Bu hatayı önlemek/çözmek için kullanarak yeniden yetkilendirin **Authorize** olduğunda düğme **belirtecinin süresi dolmadan** ve bağlı hizmeti yeniden dağıtın. Değerleri için de oluşturabilirsiniz **SessionID** ve **yetkilendirme** kullanılarak programlı bir şekilde kod gibi özellikleri:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

Bkz: [AzureDataLakeStoreLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), ve [AuthorizationSessionGetResponse sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) ayrıntıları konuları kod içinde kullanılan Data Factory sınıfları hakkında. Bir başvuru ekleyin: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll WindowsFormsWebAuthenticationDialog sınıfı. 

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL Etkinliği
Aşağıdaki JSON kod parçacığında, bir Data Lake Analytics U-SQL etkinliği ile işlem hattı tanımlar. Etkinlik tanımı, daha önce oluşturduğunuz Azure Data Lake Analytics bağlı hizmeti için bir başvuru içeriyor.   

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00Z",
        "end": "2015-08-08T01:00:00Z",
        "isPaused": false
    }
}
```

Aşağıdaki tabloda, adları ve açıklamaları bu etkinliğe özgü olan özellikleri açıklanmaktadır. 

| Özellik            | Açıklama                              | Gerekli                                 |
| :------------------ | :--------------------------------------- | :--------------------------------------- |
| type                | Type özelliği ayarlanmalıdır **DataLakeAnalyticsU SQL**. | Evet                                      |
| linkedServiceName   | Data Factory, bağlı hizmet olarak Azure Data Lake Analytics başvuru kaydedildi | Evet                                      |
| ScriptPath          | U-SQL komut dosyasını içeren klasörün yolu. Dosyanın adı büyük/küçük harfe duyarlıdır. | Hayır (komut dosyası kullanırsanız)                   |
| scriptLinkedService | Bağlantılar için veri komut dosyasını içeren depolama bağlı hizmeti | Hayır (komut dosyası kullanırsanız)                   |
| script              | ScriptPath ve scriptLinkedService belirtmek yerine satır içi betiği belirtin. Örneğin: `"script": "CREATE DATABASE test"`. | Hayır (scriptPath ve scriptLinkedService kullanıyorsanız) |
| degreeOfParallelism | Aynı anda işi çalıştırmak için kullanılan düğümlerin sayısı. | Hayır                                       |
| öncelik            | Sıraya alınan tüm önce çalıştırılması gerektiğini belirler. Alt sayısı, öncelik o kadar yüksektir. | Hayır                                       |
| parametreler          | U-SQL betiği için parametreler          | Hayır                                       |
| runtimeVersion      | Çalışma zamanı sürümünü kullanmak için U-SQL altyapısı | Hayır                                       |
| CompilationMode     | <p>U-SQL derleme modu. Şu değerlerden biri olmalıdır:</p> <ul><li>**Anlam:** Yalnızca anlam denetimleri ve gerekli sağlamlık denetimleri gerçekleştirin.</li><li>**Tam:** Sözdizimi denetimi, en iyi duruma getirme, kod oluşturma, vb. dahil olmak üzere tam derleme gerçekleştirin.</li><li>**SingleBox:** TargetType ayarıyla SingleBox tam derleme gerçekleştirin.</li></ul><p>Bu özellik için bir değer belirtmezseniz, sunucunun en iyi derleme modu belirler. </p> | Hayır                                       |

Bkz: [SearchLogProcessing.txt betik tanımı](#sample-u-sql-script) betik tanımı. 

## <a name="sample-input-and-output-datasets"></a>Örnek giriş ve çıkış veri kümesi
### <a name="input-dataset"></a>Giriş veri kümesi
Bu örnekte, giriş verileri bir Azure Data Lake Store (datalake/giriş klasörünü dosyasında searchlog.tsv dosyasını) bulunur. 

```json
{
    "name": "DataLakeTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}    
```

### <a name="output-dataset"></a>Çıktı veri kümesi
Bu örnekte U-SQL betiği tarafından üretilen çıkış verileri bir Azure Data Lake Store (datalake/çıkış klasörü) içinde depolanır. 

```json
{
    "name": "EventsByRegionTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="sample-data-lake-store-linked-service"></a>Örnek Data Lake Store bağlı hizmeti
Azure Data Lake Store bağlı hizmeti giriş/çıkış veri kümeleri tarafından kullanılan örnek tanımı aşağıda verilmiştir. 

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
        }
    }
}
```

Bkz: [Azure Data Lake Store gelen ve giden veri taşıma](data-factory-azure-datalake-connector.md) JSON özelliklerinin açıklamaları için makale. 

## <a name="sample-u-sql-script"></a>Örnek U-SQL betiği

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    TO @out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

Değerleri  **\@içinde** ve  **\@kullanıma** U-SQL betiği parametrelerinde geçirilir dinamik olarak ADF tarafından 'parameters' bölümünü kullanarak. İşlem hattı tanımındaki 'parameters' bölümüne bakın.

Azure Data Lake Analytics hizmeti üzerinde çalıştırılan işler için işlem hattı Tanımınızda degreeOfParallelism ve öncelik gibi diğer özellikleri de belirtebilirsiniz.

## <a name="dynamic-parameters"></a>Dinamik parametreler
Örnek işlem hattı tanımında, giriş ve çıkış parametreleri ile sabit kodlu değer atanır. 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

Bunun yerine dinamik parametreleri kullanmak mümkündür. Örneğin: 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

Bu durumda, giriş dosyaları hala /datalake/input klasöründen seçilir ve çıkış dosyalarının /datalake/output klasöründe oluşturulur. Dosya adları, dilim başlangıç saatine göre dinamiktir.  


