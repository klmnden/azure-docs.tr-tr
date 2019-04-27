---
title: Özel durum işleme & hata günlüğü senaryo - Azure Logic Apps | Microsoft Docs
description: İşte bir gerçek kullanım örnekleri hakkında gelişmiş özel durum işleme ve Azure Logic Apps'te günlüğe kaydetme hatası
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: hedidin
ms.author: b-hoedid
ms.reviewer: estfan, LADocs
ms.assetid: 63b0b843-f6b0-4d9a-98d0-17500be17385
ms.topic: article
ms.date: 07/29/2016
ms.openlocfilehash: 58e59e4faa135e24124f494d90437b49caa30129
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60599464"
---
# <a name="scenario-exception-handling-and-error-logging-for-logic-apps"></a>Senaryo: Logic apps için hata günlüğünü ve özel durum işleme

Bu senaryo, bir mantıksal uygulama, daha iyi özel durum işleme destekleyecek şekilde nasıl genişletebileceğinizi açıklar. Soruyu yanıtlamak için gerçek zamanlı konuşmaların kullanım örneği kullandık: "Azure Logic Apps özel durum ve hata işleme destekliyor mu?"

> [!NOTE]
> Geçerli Azure Logic Apps şemasını eylem yanıtlar için standart bir şablon sağlar. Bu şablon, hem iç doğrulama hem de bir API uygulaması döndürülen hata yanıtları içerir.

## <a name="scenario-and-use-case-overview"></a>Senaryo ve kullanım örneği genel bakış

Bu senaryo için kullanım örneği olarak Hikayeyi şöyledir: 

İyi bilinen bir sağlık hizmeti kuruluşunda bize Microsoft Dynamics CRM Online'ı kullanarak bir Hasta portalı oluşturacak bir Azure çözümü geliştirmek için gerçekleştiriliyor. Randevu kayıtları Dynamics CRM Online bir Hasta portalı ve Salesforce arasında göndermek gerekli. Biz kullanmak için sorulan [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) tüm hasta kayıtları için standart.

Projeye iki ana gereksinimlerini sahipti:  

* Dynamics CRM Online portaldan gönderilen kayıtlarını günlüğe kaydetmek için bir yöntem
* İş akışı içinde oluşan hataları görüntülemek için bir yol

> [!TIP]
> Bu proje hakkında üst düzey video için bkz. [tümleştirme kullanıcı grubu](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "tümleştirme kullanıcı grubu").

## <a name="how-we-solved-the-problem"></a>Biz sorun nasıl çözüldü

Seçtik [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/ "Azure Cosmos DB") (Cosmos DB başvuruyor kayıtları belgeleri için) günlük ve hata kayıtlar için bir depo olarak. Azure Logic Apps tüm yanıtları için standart bir şablon olduğundan, size özel bir şema oluşturun girmesi gerekmez. Bir API uygulamasına oluşturabilir **Ekle** ve **sorgu** hem hata hem de günlük kayıtları için. Biz de, bir şema için her API uygulaması içinde tanımlayabilirsiniz.  

Belirli bir tarihten sonra kayıtları temizlemek için başka bir gereksinim oluştu. Cosmos DB adlı bir özelliğe sahiptir [yaşam süresi](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "yaşam süresi") (TTL), izin verilen bize ayarlamak bir **yaşam süresi** her bir kaydı veya koleksiyon için değer. Bu özellik, el ile Cosmos DB'de kayıtları silme gereksinimini ortadan.

> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için bir Cosmos DB veritabanı ve iki koleksiyon (günlüğe kaydetme ve hataları) oluşturmanız gerekir.

## <a name="create-the-logic-app"></a>Mantıksal uygulama oluşturma

Mantıksal uygulama oluşturun ve uygulamayı Logic Apps Tasarımcısı'nda açmak için ilk adımdır bakın. Bu örnekte, üst-alt logic apps kullanıyoruz. Biz üst oluşturmuş ve bir alt mantıksal uygulama oluşturacaksınız varsayalım.

Günlük kaydı, Dynamics CRM Online oturumunu yakında kullanacağız çünkü en üstünde başlayalım. Kullanmanız gerekir bir **istek** üst mantıksal uygulama bu alt Tetikleyiciler olduğundan tetikleyin.

### <a name="logic-app-trigger"></a>Mantıksal uygulama tetikleyicisi

Kullanmakta olduğunuz bir **istek** tetiklemek aşağıdaki örnekte gösterildiği gibi:

``` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

```


## <a name="steps"></a>Adımlar

Biz, Dynamics CRM Online portaldan hasta kaydının (istek) kaynak oturum açmalısınız.

1. Size yeni bir randevu kayıt Dynamics CRM Online'dan almanız gerekir.

   Bizimle CRM'den yakında tetikleyici sağlar **CRM PatentId**, **kayıt türü**, **yeni veya güncelleştirilmiş kayıt** (yeni veya Boolean değeri güncelleştirin), ve  **SalesforceId**. **SalesforceId** yalnızca bir güncelleştirme için kullanıldığından null olabilir.
   CRM kullanarak CRM Kayıt aldığımız **PatientID** ve **kayıt türü**.

2. Ardından, Azure Cosmos DB SQL API uygulamamızı eklemek ihtiyacımız **InsertLogEntry** Logic Apps Tasarımcısı'nda burada gösterildiği gibi işlem.

   **Günlük Girdisi Ekle**

   ![Günlük Girdisi Ekle](media/logic-apps-scenario-error-and-exception-handling/lognewpatient.png)

   **Hata Girişi Ekle**

   ![Günlük Girdisi Ekle](media/logic-apps-scenario-error-and-exception-handling/insertlogentry.png)

   **Onay için kayıt hatası oluşturun**

   ![Koşul](media/logic-apps-scenario-error-and-exception-handling/condition.png)

## <a name="logic-app-source-code"></a>Mantıksal uygulama kaynak kodu

> [!NOTE]
> Aşağıdaki örnek yalnızca örnektir. Bu öğreticide uygulama artık üretim, değerini bağlı olduğu bir **kaynak düğüm** bir randevu. zamanlamayla ilgili özellikleri görüntülenmeyebilir > 

### <a name="logging"></a>Günlüğe kaydetme

Aşağıdaki mantıksal uygulama kod örneği, günlüğe kaydetme nasıl ele alınacağını gösterir.

#### <a name="log-entry"></a>Günlük girişi

Aşağıda, bir günlük girişi eklemek için mantıksal uygulama kaynak kodu verilmiştir.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Günlük istek

API uygulamasına gönderilen günlük istek iletisi aşağıda verilmiştir.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{/"Pragma/":/"no-cache/",/"x-ms-request-id/":/"e750c9a9-bd48-44c4-bbba-1688b6f8a132/",/"OData-Version/":/"4.0/",/"Cache-Control/":/"no-cache/",/"Date/":/"Fri, 10 Jun 2016 22:31:56 GMT/",/"Set-Cookie/":/"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1/",/"Server/":/"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0/",/"X-AspNet-Version/":/"4.0.30319/",/"X-Powered-By/":/"ASP.NET/",/"Content-Length/":/"1935/",/"Content-Type/":/"application/json; odata.metadata=minimal; odata.streaming=true/",/"Expires/":/"-1/"}"
        }
    }

```


#### <a name="log-response"></a>Günlük yanıt

API uygulamasında oturum yanıt iletisi aşağıda verilmiştir.

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "/"0400fc2f-0000-0000-0000-575b3ff00000/"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{/"Pragma/":/"no-cache/",/"x-ms-request-id/":/"e750c9a9-bd48-44c4-bbba-1688b6f8a132/",/"OData-Version/":/"4.0/",/"Cache-Control/":/"no-cache/",/"Date/":/"Fri, 10 Jun 2016 22:31:56 GMT/",/"Set-Cookie/":/"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1/",/"Server/":/"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0/",/"X-AspNet-Version/":/"4.0.30319/",/"X-Powered-By/":/"ASP.NET/",/"Content-Length/":/"1935/",/"Content-Type/":/"application/json; odata.metadata=minimal; odata.streaming=true/",/"Expires/":/"-1/"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Artık adımları işleme hatası bakalım.

### <a name="error-handling"></a>Hata işleme

Hata işleme nasıl uygulayacağınıza dair aşağıdaki mantıksal uygulama kod örneği gösterilmektedir.

#### <a name="create-error-record"></a>Hata kayıt oluşturma

Burada, bir hata kaydı oluşturmak için mantıksal uygulama kaynak kodu verilmiştir.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}             
```

#### <a name="insert-error-into-cosmos-db--request"></a>Ekleme hatası Cosmos DB'ye--iste

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MasterID_mp__c/":/"/",/"C_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"ONY_ID__c/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-cosmos-db--response"></a>Cosmos DB'ye--yanıt ekleme hatası

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "/"0c00eaac-0000-0000-0000-575b3fdc0000/"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/":/"DO - Degree level is DO/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MterID_mp__c/":/"/",/"Medicis_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"XXXXXXX/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>Salesforce hata yanıtı

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="return-the-response-back-to-parent-logic-app"></a>Yanıt üst mantıksal uygulama döndürür

Yanıt aldıktan sonra yanıt üst mantıksal uygulama geçirebilirsiniz.

#### <a name="return-success-response-to-parent-logic-app"></a>Başarılı yanıt üst mantıksal uygulamaya döndürür

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "    Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-parent-logic-app"></a>Üst mantıksal uygulama için hata yanıtı döndürür

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="cosmos-db-repository-and-portal"></a>Cosmos DB depo ve portal

Çözümümüz eklenen özellikleriyle [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db).

### <a name="error-management-portal"></a>Yönetim Portalı'nı hata

Hataları görüntülemek için Cosmos DB'den hata kayıtları görüntülemek için bir MVC web uygulaması oluşturabilirsiniz. **Listesi**, **ayrıntıları**, **Düzenle**, ve **Sil** işlemleri'nın geçerli sürümünde dahil edilir.

> [!NOTE]
> İşlemi Düzenle: Cosmos DB, tüm belgeyi değiştirir. Gösterilen kayıtları **listesi** ve **ayrıntı** örnekleri yalnızca görünümleridir. Bunlar gerçek hasta randevu kayıtlarını değildir.

Daha önce açıklanan yaklaşımı ile oluşturulan bizim MVC uygulama ayrıntılarını örnekleri aşağıda verilmiştir.

#### <a name="error-management-list"></a>Hata Yönetimi listesi
![Hata Listesi](media/logic-apps-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Hata Yönetimi ayrıntı görünümü
![Hata Ayrıntıları](media/logic-apps-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Günlük Yönetim Portalı

Günlükleri görüntülemek için de bir MVC web uygulaması oluşturduk. Daha önce açıklanan yaklaşımı ile oluşturulan bizim MVC uygulama ayrıntılarını örnekleri aşağıda verilmiştir.

#### <a name="sample-log-detail-view"></a>Örnek günlük ayrıntı görünümü
![Günlük ayrıntı görünümü](media/logic-apps-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>API uygulama ayrıntıları

#### <a name="logic-apps-exception-management-api"></a>Logic Apps özel durum yönetimi API'si

Burada açıklandığı gibi açık kaynaklı Azure Logic Apps özel durum yönetimi API'si uygulamamızı işlevsellik sağlar - iki denetleyicisi vardır:

* **ErrorController** bir hata kaydı (belge), bir Azure Cosmos DB koleksiyonunda ekler.
* **LogController** günlük kaydı (belge), bir Azure Cosmos DB koleksiyonunda ekler.

> [!TIP]
> Her iki denetleyicilerinin kullandığı `async Task<dynamic>` işlemleri, Azure Cosmos DB şema işlemi gövdesinde oluşturabilmesi için çalışma zamanında çözümlenecek işlemlere izin verme. 
> 

Azure Cosmos DB'de her belgenin benzersiz bir kimliği olması gerekir Kullanıyoruz `PatientId` ve Unix zaman damgası değeri (çift) dönüştürülür bir zaman damgası ekleme. Kesirli değer kaldırmak için değer olacak şekilde kısaltın.

Kaynak kodu hata denetleyicimizin API görüntüleyebileceğiniz [GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/LogicAppsExceptionManagementApi/Controllers/LogController.cs).

API'yi bir mantıksal uygulamadan aşağıdaki söz dizimini kullanarak diyoruz:

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

Yukarıdaki kod örneğinde ifadesinde denetler *Create_NewPatientRecord* durumunu **başarısız**.

## <a name="summary"></a>Özet

* Günlüğe kaydetme ve hata işleme bir mantıksal uygulama çalıştırmasında kolayca uygulayabilirsiniz.
* Azure Cosmos DB, günlük ve hata kayıtları (belgeler) için deposu olarak kullanabilirsiniz.
* MVC, günlük ve hata kayıtları görüntülemek için bir portal oluşturmak için kullanabilirsiniz.

### <a name="source-code"></a>Kaynak kod

Logic Apps özel durum yönetimi API uygulaması için kaynak kodu bu kullanılabilir [GitHub deposu](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "mantıksal uygulama özel durum yönetimi API'si").

## <a name="next-steps"></a>Sonraki adımlar

* [Daha fazla mantıksal uygulama örnekleri ve senaryoları görüntüleyin](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Mantıksal uygulamaları izleme hakkında bilgi edinin](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Mantıksal uygulamalar için otomatik dağıtım şablonları oluşturma](../logic-apps/logic-apps-create-deploy-template.md)
