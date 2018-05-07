---
title: Azure Data Factory etkinliğinde Web | Microsoft Docs
description: Bir REST uç noktasından bir ardışık düzen çağrılacak Web etkinlik, bir Data Factory ile desteklenen kontrol akışı etkinlikleri nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: shlo
ms.openlocfilehash: 58faed48f5031b26f1340f3766fdd8bdc6bd2ccb
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="web-activity-in-azure-data-factory"></a>Azure Data Factory Web etkinlik
Web Etkinliği bir Data Factory işlem hattından özel bir REST uç noktasını çağırmak için kullanılabilir. Etkinlik tarafından kullanılacak ve erişilecek veri kümelerini ve bağlı hizmetleri geçirebilirsiniz. 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 belgeleri](v1/data-factory-introduction.md) konusunu inceleyin.

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
ad | Web etkinlik adı | Dize | Evet
type | Ayarlanmalıdır **WebActivity**. | Dize | Evet
method | Hedef uç nokta için REST API yöntemi. | Dize. <br/><br/>Desteklenen türleri: "GET", "POST", "PUT" | Evet
url | Hedef uç noktası ve yol | Dize (veya dize Resulttype'a sahip ifadesi). Etkinlik olacak zaman aşımı hatasıyla 1 dakika, uç noktasından bir yanıt alamazsa. | Evet
headers | İsteği gönderilir üstbilgileri. Örneğin, bir isteği dil ve türünü ayarlamak için: `"headers" : { "Accept-Language": "en-us", "Content-Type": "application/json" }`. | Dize (veya dize Resulttype'a sahip ifade) | Evet, Content-type üstbilgisi gereklidir. `"headers":{ "Content-Type":"application/json"}`
body | Uç noktasına gönderilen yükünü temsil eder. POST/PUT yöntemleri için gereklidir.  | Dize (veya dize Resulttype'a sahip ifadesi). <br/><br/>İstek yükünde şeması bkz [istek yükü şeması](#request-payload-schema) bölümü. | Hayır
kimlik doğrulaması | Uç nokta çağırmak için kullanılan kimlik doğrulama yöntemi. Desteklenen türler şunlardır: "Basic ya ClientCertificate." Daha fazla bilgi için bkz: [kimlik doğrulaması](#authentication) bölümü. Kimlik doğrulama gerekli değilse, bu özellik dışlayın. | Dize (veya dize Resulttype'a sahip ifade) | Hayır
Veri kümeleri | Veri kümeleri listesini uç noktasına geçirildi. | Veri kümesi başvuruları dizisi. Boş bir dizi olabilir. | Evet
linkedServices | Bağlı hizmetler listesi uç noktasına geçirildi. | Bağlantılı hizmeti başvuruları dizisi. Boş bir dizi olabilir. | Evet

> [!NOTE]
> Web etkinliği çağırır REST uç noktalarını bir yanıt türü JSON döndürmesi gerekir. Etkinlik olacak zaman aşımı hatasıyla 1 dakika, uç noktasından bir yanıt alamazsa.

Aşağıdaki tabloda JSON içeriği için gereksinimleri gösterilmektedir:

| Değer türü | İstek gövdesi | Yanıt gövdesi |
|---|---|---|
|JSON nesnesi | Desteklenen | Desteklenen |
|JSON dizisi | Desteklenen <br/>(Şu anda bir hata sonucu olarak JSON dizileri çalışmıyor. Bir düzeltme devam ediyor.) | Desteklenmiyor |
| JSON değeri | Desteklenen | Desteklenmiyor |
| Olmayan JSON türü | Desteklenmiyor | Desteklenmiyor |
||||

## <a name="authentication"></a>Kimlik Doğrulaması

### <a name="none"></a>None
Kimlik doğrulama gerekli değilse, "kimlik doğrulaması" özelliğini içermez.

### <a name="basic"></a>Temel
Temel kimlik doğrulaması ile kullanmak için kullanıcı adı ve parola belirtin. 

```json
"authentication":{  
   "type":"Basic,
   "username":"****",
   "password":"****"
}
```

### <a name="client-certificate"></a>İstemci sertifikası
Base64 ile kodlanmış içeriği bir PFX dosyası ve parola belirtin. 

```json
"authentication":{  
   "type":"ClientCertificate",
   "pfx":"****",   
   "password":"****"
}
```
## <a name="request-payload-schema"></a>İstek yükü şeması
POST/PUT yöntemini kullandığınızda, gövde özelliği uç noktasına gönderilen yükünü temsil eder. Bağlı hizmetler ve veri kümelerini yükü bir parçası olarak geçirebilirsiniz. Yükü için şema şöyledir: 

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
Bu örnekte, ardışık düzen web etkinliğinde bir REST uç noktası çağırır. Bir Azure SQL bağlı hizmeti ve bir Azure SQL veri kümesi için uç nokta geçirir. REST uç noktası Azure SQL Server'a bağlanmak için Azure SQL bağlantı dizesini kullanır ve SQL server örneğinin adını döndürür. 

### <a name="pipeline-definition"></a>Ardışık düzen tanımı

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

### <a name="pipeline-parameter-values"></a>Ardışık Düzen parametre değerleri

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
Data Factory ile desteklenen diğer denetim akışı etkinlikleri bakın: 

- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
