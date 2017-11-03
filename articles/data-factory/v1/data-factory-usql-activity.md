---
title: "U-SQL betiği - Azure kullanarak veri dönüştürme | Microsoft Docs"
description: "Azure Data Lake Analytics işlem hizmette U-SQL betiklerini çalıştırarak nasıl işleneceğini veya dönüştürme veri öğrenin."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e17c1255-62c2-4e2e-bb60-d25274903e80
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 3a0a097afa0ef5efe11cb5044bf9ea5d399e463f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Üzerinde Azure Data Lake Analytics U-SQL betiklerini çalıştırarak veri dönüştürme 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-usql-activity.md)
> * [Sürüm 2 - Önizleme](../transform-data-using-data-lake-analytics.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 U-SQL etkinliği](../transform-data-using-data-lake-analytics.md).

Bir Azure data factory işlem hattı bağlantılı işlem hizmetlerini kullanarak bağlantılı depolama Hizmetleri'nde verilerini işler. Burada her etkinlik özel işleme işlemi gerçekleştiren etkinlikler dizisini içerir. Bu makalede **Data Lake Analytics U-SQL etkinliği** çalıştıran bir **U-SQL** üzerinde komut dosyası bir **Azure Data Lake Analytics** bağlantılı hizmet işlem. 

Bir Azure Data Lake Analytics hesabı, bir veri Lake Analytics U-SQL etkinliği ile işlem hattı oluşturmadan önce oluşturun. Azure Data Lake Analytics hakkında bilgi edinmek için [Azure Data Lake Analytics ile çalışmaya başlama](../../data-lake-analytics/data-lake-analytics-get-started-portal.md).

Gözden geçirme [ilk ardışık düzen öğreticinizi yapı](data-factory-build-your-first-pipeline.md) data factory oluşturmak ayrıntılı adımlar için Hizmetleri, veri kümeleri ve işlem hattı bağlı. JSON parçacığı, Data Factory varlıklarını oluşturmak için Data Factory Düzenleyici veya Visual Studio veya Azure PowerShell ile kullanın.

## <a name="supported-authentication-types"></a>Desteklenen kimlik doğrulama türleri
Data Lake Analytics karşı kimlik doğrulama türleri aşağıdaki U-SQL etkinliği destekler:
* Hizmet sorumlusu kimlik doğrulaması
* Kullanıcı kimlik bilgisi (OAuth) kimlik doğrulaması 

Zamanlanmış U-SQL yürütmesi için özellikle bir hizmet asıl kimlik doğrulaması kullanmanızı öneririz. Belirteç süre sonu davranış kullanıcı kimlik bilgilerinin ile ortaya çıkabilir. Yapılandırma ayrıntıları için bkz: [bağlantılı hizmet özellikleri](#azure-data-lake-analytics-linked-service) bölümü.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics hizmeti bağlı
Oluşturduğunuz bir **Azure Data Lake Analytics** bir Azure Data Lake Analytics bağlamak için bağlantılı hizmeti bir Azure data factory hizmetine işlem. Data Lake Analytics U-SQL etkinliği ardışık düzeninde bu bağlı hizmetin başvuruyor. 

Aşağıdaki tabloda JSON tanımında kullanılan genel özellikleri için açıklamalar sağlanır. Daha fazla hizmet sorumlusu ve kullanıcı kimlik bilgileri doğrulaması arasında seçim yapabilirsiniz.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **türü** |Type özelliği ayarlanmalıdır: **AzureDataLakeAnalytics**. |Evet |
| **accountName** |Azure Data Lake Analytics hesap adı. |Evet |
| **dataLakeAnalyticsUri** |Azure Data Lake Analytics URI. |Hayır |
| **Subscriptionıd** |Azure abonelik kimliği |Hayır (belirtilmezse, data Factory abonelik kullanılır). |
| **resourceGroupName** |Azure kaynak grubu adı |Hayır (belirtilmezse, kaynak grubu data Factory kullanılır). |

### <a name="service-principal-authentication-recommended"></a>(Önerilen) hizmet asıl kimlik doğrulaması
Hizmet asıl kimlik doğrulaması kullanmak için Azure Active Directory (Azure AD) bir uygulama varlığı kaydetmek ve Data Lake Store'a erişim izni. Ayrıntılı adımlar için bkz: [hizmeti için kimlik doğrulama](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Bağlantılı hizmet tanımlamak için kullandığınız aşağıdaki değerleri not edin:
* Uygulama Kimliği
* Uygulama anahtarı 
* Kiracı Kimliği

Hizmet asıl kimlik doğrulaması, aşağıdaki özellikleri belirterek kullanın:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **servicePrincipalId** | Uygulamanın istemci kimliği belirtin. | Evet |
| **servicePrincipalKey** | Uygulamanın anahtarını belirtin. | Evet |
| **Kiracı** | Uygulamanızın bulunduğu altında Kiracı bilgileri (etki alanı adı veya Kiracı kimliği) belirtin. Azure portalının sağ üst köşedeki fare gelerek alabilir. | Evet |

**Örnek: Hizmet asıl kimlik doğrulaması**
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
| **Yetkilendirme** | Tıklatın **Authorize** Data Factory Düzenleyici'düğmesine tıklayın ve bu özelliği otomatik olarak oluşturulur yetkilendirme URL'si atar kimlik bilgilerinizi girin. | Evet |
| **SessionID** | OAuth yetkilendirme oturumundan OAuth oturum kimliği. Her oturum kimliği benzersiz olup yalnızca bir kez kullanılabilir. Data Factory Düzenleyici kullandığınızda bu ayarı otomatik olarak oluşturulur. | Evet |

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
Oluşturulan kullanarak Yetkilendirme kodu **Authorize** düğmesi süre sonra süresi dolar. Farklı türlerdeki kullanıcı hesapları için zaman aşımı süresinin için aşağıdaki tabloya bakın. Aşağıdaki hatayı görebilirsiniz ne zaman ileti kimlik doğrulama **belirtecinin süresi dolmadan**: kimlik bilgisi işlemi hatası: invalid_grant - AADSTS70002: Kimlik doğrulanırken hata oluştu. AADSTS70008: Sağlanan erişim izninin süresi doldu veya iptal edildi. İzleme kimliği: d18629e8-af88-43c5-88e3-d8419eb1fca1 bağıntı kimliği: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 zaman damgası: 2015-12-15 21:09:31Z

| Kullanıcı türü | Kullanım süresi sonu |
|:--- |:--- |
| Azure Active Directory tarafından yönetilmeyen kullanıcı hesapları (@hotmail.com, @live.comvb..) |12 saat |
| Azure Active Directory (AAD tarafından) yönetilen kullanıcı hesapları |14 gün sonra en son dilim çalıştırın. <br/><br/>90 OAuth tabanlı bağlantılı hizmette dayanan bir dilim 14 günde bir en az bir kez çalıştırılıyorsa, gün. |

Bu hatayı önlemek/çözmek için kullanarak yeniden yetkilendirin **Authorize** ne zaman düğmesini **belirtecinin süresi dolmadan** ve bağlantılı hizmeti yeniden dağıtın. Değerleri de oluşturabilirsiniz **SessionID** ve **yetkilendirme** kullanılarak programlı olarak kod şu şekilde özellikleri:

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

Bkz: [AzureDataLakeStoreLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), ve [AuthorizationSessionGetResponse sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) konuları kod içinde kullanılan veri fabrikası sınıfları hakkında ayrıntılı bilgi için. Bir başvuru ekleyin: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll WindowsFormsWebAuthenticationDialog sınıfı için. 

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL Etkinliği
Aşağıdaki JSON parçacığında, bir Data Lake Analytics U-SQL etkinliği ile işlem hattı tanımlar. Etkinlik tanımı daha önce oluşturduğunuz Azure Data Lake Analytics bağlantılı hizmeti bir başvuru içeriyor.   

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

Aşağıdaki tabloda, adları ve açıklamaları bu etkinliğe özgü özellikleri açıklanmaktadır. 

| Özellik            | Açıklama                              | Gerekli                                 |
| :------------------ | :--------------------------------------- | :--------------------------------------- |
| type                | Type özelliği ayarlamak **DataLakeAnalyticsU SQL**. | Evet                                      |
| linkedServiceName   | Veri fabrikasında bağlı hizmet olarak Azure Data Lake Analytics referansı kayıtlı | Evet                                      |
| scriptPath          | U-SQL komut dosyasını içeren klasörün yolu. Dosyanın adı büyük/küçük harf duyarlıdır. | Hayır (komut dosyası kullanırsanız)                   |
| scriptLinkedService | Veri Fabrikası için komut dosyasını içeren depolamayı bağlı hizmet | Hayır (komut dosyası kullanırsanız)                   |
| Komut dosyası              | ScriptPath ve scriptLinkedService belirtme yerine satır içi betiği belirtin. Örneğin: `"script": "CREATE DATABASE test"`. | Hayır (scriptPath ve scriptLinkedService kullanıyorsanız) |
| degreeOfParallelism | Aynı anda işi çalıştırmak için kullanılan düğümlerin sayısı. | Hayır                                       |
| Öncelik            | İlk çalıştırmak için sıraya alınan tüm işlerden seçili belirler. Alt sayısı, öncelik o kadar yüksektir. | Hayır                                       |
| parametreler          | U-SQL betiği için parametreler          | Hayır                                       |
| runtimeVersion      | Çalışma zamanı sürümü kullanmak için U-SQL | Hayır                                       |
| compilationMode     | <p>U-SQL derleme modu. Şu değerlerden biri olmalıdır:</p> <ul><li>**Anlam:** yalnızca anlamsal denetler ve gerekli sağlamlık denetimleri gerçekleştirin.</li><li>**Tam:** sözdizimi denetimi, en iyi duruma getirme, kod oluşturma, vb. dahil olmak üzere tam derleme gerçekleştirin.</li><li>**SingleBox:** SingleBox için TargetType ayarıyla tam derleme gerçekleştirin.</li></ul><p>Bu özellik için bir değer belirtmezseniz, sunucu en iyi bir derleme moduna belirler. </p> | Hayır                                       |

Bkz: [SearchLogProcessing.txt betik tanımı](#sample-u-sql-script) betik tanımı. 

## <a name="sample-input-and-output-datasets"></a>Örnek girdi ve çıktı veri kümeleri
### <a name="input-dataset"></a>Girdi veri kümesi
Bu örnekte, bir Azure Data Lake Store (datalake/giriş klasörünü dosyasında searchlog.tsv dosyasını) giriş verisi bulunur. 

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
Bu örnekte U-SQL komut dosyası tarafından üretilen çıktı verilerini bir Azure Data Lake Store'da (datalake/çıkış klasörü) depolanır. 

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

### <a name="sample-data-lake-store-linked-service"></a>Bağlantılı hizmetinin örnek veri Gölü deposu
Azure Data Lake Store giriş/çıkış veri kümeleri tarafından kullanılan hizmet bağlı örnek tanımı aşağıda verilmiştir. 

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

Bkz: [Azure Data Lake Store gelen ve veri taşıma](data-factory-azure-datalake-connector.md) makale açıklamaları JSON özellikleri için. 

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

Değerleri  **@in**  ve  **@out**  U-SQL betiği parametrelerinde geçirilir dinamik olarak tarafından ADF 'parameters' bölümünü kullanarak. Ardışık Düzen tanımında 'parameters' bölümüne bakın.

DegreeOfParallelism ve öncelik gibi diğer özellikleri ardışık düzen tanımınızı Azure Data Lake Analytics hizmeti Çalıştır işleri için de belirtebilirsiniz.

## <a name="dynamic-parameters"></a>Dinamik parametreler
Örnek ardışık düzen tanımı'nda giriş ve çıkış parametreleri sabit kodlanmış değerlerle atandı. 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

Dinamik parametreleri bunun yerine kullanmak da mümkündür. Örneğin: 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

Bu durumda, giriş dosyaları hala /datalake/input klasöründen toplanmış ve çıktı dosyaları /datalake/output klasöründe oluşturulur. Dosya adları dilim başlangıç zamanı temel alınarak dinamik.  


