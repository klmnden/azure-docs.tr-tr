---
title: Web etkinliği Azure Data factory'de | Microsoft Docs
description: Bir REST uç noktasından bir işlem hattı çağıracak Web etkinliği bir Data Factory tarafından desteklenen denetim akışı etkinlikleri nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: shlo
ms.openlocfilehash: 7edaa4c673c2cb94dc5bd0245ce66c9fe6a7dd3c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60764297"
---
# <a name="web-activity-in-azure-data-factory"></a>Azure Data factory'de Web etkinliği
Web Etkinliği bir Data Factory işlem hattından özel bir REST uç noktasını çağırmak için kullanılabilir. Etkinlik tarafından kullanılacak ve erişilecek veri kümelerini ve bağlı hizmetleri geçirebilirsiniz.

## <a name="syntax"></a>Sözdizimi

```json
{
   "name":"MyWebActivity",
   "type":"WebActivity",
   "typeProperties":{
      "method":"Post",
      "url":"<URLEndpoint>",
      "headers":{
         "Content-Type":"application/json"
      },
      "authentication":{
         "type":"ClientCertificate",
         "pfx":"****",
         "password":"****"
      },
      "datasets":[
         {
            "referenceName":"<ConsumedDatasetName>",
            "type":"DatasetReference",
            "parameters":{
               ...
            }
         }
      ],
      "linkedServices":[
         {
            "referenceName":"<ConsumedLinkedServiceName>",
            "type":"LinkedServiceReference"
         }
      ]
   }
}

```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
ad | Web etkinliği adı | String | Evet
type | Ayarlanmalıdır **WebActivity**. | String | Evet
method | Hedef uç nokta için REST API yöntemi. | dize. <br/><br/>Desteklenen türler: "POST", "PUT GET" | Evet
url | Hedef uç nokta ve yolu | Dize (veya dizenin ifadenin resulttype'ı ile). Etkinlik yapmayacağınıza zaman aşımı 1 dakika ile bir hata, uç noktasından bir yanıt almaz. | Evet
Üst bilgileri | Gönderilen istek için üstbilgiler. Örneğin dil ve türdeki bir istek üzerinde ayarlanan: `"headers" : { "Accept-Language": "en-us", "Content-Type": "application/json" }`. | Dize (veya dizenin ifadenin resulttype'ı ile) | Evet, Content-type üst bilgisi gereklidir. `"headers":{ "Content-Type":"application/json"}`
body | Uç noktaya gönderdi yükünü temsil eder.  | Dize (veya dizenin ifadenin resulttype'ı ile). <br/><br/>İstek yükü şemayı [istek yükü şeması](#request-payload-schema) bölümü. | POST/PUT yöntemleri için gereklidir.
kimlik doğrulaması | Uç noktasını çağırmak için kullanılan kimlik doğrulama yöntemi. "Temel veya ClientCertificate." türleri desteklenir Daha fazla bilgi için [kimlik doğrulaması](#authentication) bölümü. Kimlik doğrulama gerekli değilse, bu özellik hariç tutun. | Dize (veya dizenin ifadenin resulttype'ı ile) | Hayır
veri kümeleri | Veri kümelerinin listesini uç noktasına geçilen. | Veri kümesi yapılan başvuruların dizisi. Boş bir dizi olabilir. | Evet
linkedServices | Bağlı hizmetler listesini uç noktasına geçilen. | Bağlı hizmet başvuruları dizisi. Boş bir dizi olabilir. | Evet

> [!NOTE]
> Web etkinliği çağırır REST uç noktalarını türü JSON yanıtı döndürmelidir. Etkinlik yapmayacağınıza zaman aşımı 1 dakika ile bir hata, uç noktasından bir yanıt almaz.

JSON içeriği gereksinimleri aşağıdaki tabloda gösterilmiştir:

| Değer türü | İstek gövdesi | Yanıt gövdesi |
|---|---|---|
|JSON nesnesi | Desteklenen | Desteklenen |
|JSON dizisi | Desteklenen <br/>(Şu anda bir hata sonucu olarak JSON dizileri çalışmaz. Düzeltme sürüyor.) | Desteklenmiyor |
| JSON değeri | Desteklenen | Desteklenmiyor |
| JSON olmayan türü | Desteklenmiyor | Desteklenmiyor |
||||

## <a name="authentication"></a>Kimlik Doğrulaması

### <a name="none"></a>None
Kimlik doğrulama gerekli değilse, "kimlik doğrulaması" özelliğini içermez.

### <a name="basic"></a>Temel
Kullanıcı adı ve temel kimlik doğrulaması ile kullanılacak parolayı belirtin.

```json
"authentication":{
   "type":"Basic",
   "username":"****",
   "password":"****"
}
```

### <a name="client-certificate"></a>İstemci sertifikası
Base64 ile kodlanmış içeriği bir PFX dosyası ve parolayı belirtin.

```json
"authentication":{
   "type":"ClientCertificate",
   "pfx":"****",
   "password":"****"
}
```

### <a name="managed-identity"></a>Yönetilen Kimlik

Erişim belirteci için istenecektir yönetilen kimlik kullanarak veri fabrikası için Kaynak URI belirtin. Azure Resource Management API'si çağırmak için kullanın `https://management.azure.com/`. Yönetilen kimlikleri hakkında daha fazla bilgi Bkz çalıştığı için [yönetilen Azure kaynaklarına genel bakış sayfası için kimlikleri](/azure/active-directory/managed-identities-azure-resources/overview).

```json
"authentication": {
    "type": "MSI",
    "resource": "https://management.azure.com/"
}
```

## <a name="request-payload-schema"></a>İstek yükü şeması
POST/PUT yöntemini kullandığınızda, gövdesi özelliğinden uç noktaya gönderdi yükünü temsil eder. Bağlı hizmetleri ve veri kümeleri yükün bir parçası olarak geçirebilirsiniz. Yükü şeması şu şekildedir:

```json
{
    "body": {
        "myMessage": "Sample",
        "datasets": [{
            "name": "MyDataset1",
            "properties": {
                ...
            }
        }],
        "linkedServices": [{
            "name": "MyStorageLinkedService1",
            "properties": {
                ...
            }
        }]
    }
}
```

## <a name="example"></a>Örnek
Bu örnekte, bir REST uç noktası işlem hattının web etkinliği çağırır. Bu seçenek, bir Azure SQL bağlı hizmeti ve bir Azure SQL veri kümesi için uç nokta geçirir. REST uç noktası, Azure SQL sunucusuna bağlanmak için Azure SQL bağlantı dizesini kullanır ve SQL server örneğinin adını döndürür.

### <a name="pipeline-definition"></a>İşlem hattı

```json
{
    "name": "<MyWebActivityPipeline>",
    "properties": {
        "activities": [
            {
                "name": "<MyWebActivity>",
                "type": "WebActivity",
                "typeProperties": {
                    "method": "Post",
                    "url": "@pipeline().parameters.url",
                    "headers": {
                        "Content-Type": "application/json"
                    },
                    "authentication": {
                        "type": "ClientCertificate",
                        "pfx": "*****",
                        "password": "*****"
                    },
                    "datasets": [
                        {
                            "referenceName": "MySQLDataset",
                            "type": "DatasetReference",
                            "parameters": {
                                "SqlTableName": "@pipeline().parameters.sqlTableName"
                            }
                        }
                    ],
                    "linkedServices": [
                        {
                            "referenceName": "SqlLinkedService",
                            "type": "LinkedServiceReference"
                        }
                    ]
                }
            }
        ],
        "parameters": {
            "sqlTableName": {
                "type": "String"
            },
            "url": {
                "type": "String"
            }
        }
    }
}

```

### <a name="pipeline-parameter-values"></a>İşlem hattı parametre değerleri

```json
{
    "sqlTableName": "department",
    "url": "https://adftes.azurewebsites.net/api/execute/running"
}

```

### <a name="web-service-endpoint-code"></a>Web Hizmeti uç noktası kodu

```csharp

[HttpPost]
public HttpResponseMessage Execute(JObject payload)
{
    Trace.TraceInformation("Start Execute");

    JObject result = new JObject();
    result.Add("status", "complete");

    JArray datasets = payload.GetValue("datasets") as JArray;
    result.Add("sinktable", datasets[0]["properties"]["typeProperties"]["tableName"].ToString());

    JArray linkedServices = payload.GetValue("linkedServices") as JArray;
    string connString = linkedServices[0]["properties"]["typeProperties"]["connectionString"].ToString();

    System.Data.SqlClient.SqlConnection sqlConn = new System.Data.SqlClient.SqlConnection(connString);

    result.Add("sinkServer", sqlConn.DataSource);

    Trace.TraceInformation("Stop Execute");

    return this.Request.CreateResponse(HttpStatusCode.OK, result);
}

```

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinlikleri bakın:

- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
