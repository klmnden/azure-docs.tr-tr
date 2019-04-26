---
title: Giden bağlantı kimlik doğrulaması - Azure Zamanlayıcı
description: Ayarlamak veya kaldırmak için Azure Scheduler giden bağlantı kimlik doğrulaması hakkında bilgi edinin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 6707f82b-7e32-401b-a960-02aae7bb59cc
ms.topic: article
ms.date: 08/15/2016
ms.openlocfilehash: 42d6ec93a3382f494b49fb574c4aee5e8eec142a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60344357"
---
# <a name="outbound-authentication-for-azure-scheduler"></a>Azure Zamanlayıcı giden bağlantı kimlik doğrulaması

> [!IMPORTANT]
> Kullanımdan kaldırılan Azure Scheduler uygulamasının yerini [Azure Logic Apps](../logic-apps/logic-apps-overview.md) alacaktır. İş zamanlamak için [Azure Logic Apps'ı deneyebilirsiniz](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

Azure zamanlayıcı işleri, diğer Azure Hizmetleri, Salesforce.com, Facebook ve güvenli bir özel Web siteleri gibi kimlik doğrulaması gerektiren hizmetleri çağıran gerekebilir. Çağrılan hizmet Scheduler işi istenen kaynaklara erişebileceğini belirleyebilirsiniz. 

Zamanlayıcı, bu kimlik doğrulaması modelleri destekler: 

* *İstemci sertifikası* SSL/TLS istemci sertifikalarını kullanarak kimlik doğrulaması
* *Temel* kimlik doğrulaması
* *Active Directory OAuth* kimlik doğrulaması

## <a name="add-or-remove-authentication"></a>Ekleme veya kaldırma kimlik doğrulaması

* Oluşturduğunuzda veya işi güncelleştirmek için Scheduler işi, kimlik doğrulaması eklemek için Ekle `authentication` JavaScript nesne gösterimi (JSON) alt öğeye `request` öğesi. 

  Yanıtları Zamanlayıcı hizmeti için PUT, PATCH veya POST isteğinde geçirilecek gizli anahtarları asla geri dönmemek `authentication` nesne. 
  Yanıtlar, gizli bilgileri null olarak ayarlayın veya kimliği doğrulanmış varlığı temsil eden bir ortak belirteci kullanabilir. 

* Kimlik doğrulaması bir zamanlayıcı işten kaldırmak için açıkça PUT veya PATCH isteği iş üzerinde çalıştırın ve ayarlama `authentication` nesne null. Yanıt, tüm kimlik doğrulaması özelliklerini içermez.

## <a name="client-certificate"></a>İstemci sertifikası

### <a name="request-body---client-certificate"></a>İstek gövdesi - istemci sertifikası

Kimlik doğrulaması kullanarak eklerken `ClientCertificate` model, bu ek öğeler istek gövdesinde belirtin.  

| Öğe | Gerekli | Açıklama |
|---------|----------|-------------|
| **kimlik doğrulaması** (üst öğe) | SSL istemci sertifikası kullanılarak kimlik doğrulaması nesnesi |
| **type** | Evet | Kimlik doğrulaması türü. SSL istemci sertifikaları için değerdir `ClientCertificate`. |
| **PFX** | Evet | PFX dosyasının base64 ile kodlanmış içeriği |
| **Parola** | Evet | PFX dosyasına erişim için parola |
||| 

### <a name="response-body---client-certificate"></a>Yanıt gövdesi - istemci sertifikası 

Yanıt, kimlik doğrulama bilgilerini bir istek gönderildiğinde, bu kimlik doğrulama öğeleri içerir.

| Öğe | Açıklama | 
|---------|-------------| 
| **kimlik doğrulaması** (üst öğe) | SSL istemci sertifikası kullanılarak kimlik doğrulaması nesnesi |
| **type** | Kimlik doğrulaması türü. SSL istemci sertifikaları için değerdir `ClientCertificate`. |
| **certificateThumbprint** |Sertifikanın parmak izi |
| **CertificateSubjectName** |Sertifika konu ayırt edici ad |
| **certificateExpiration** | Sertifikanın sona erme tarihi |
||| 

### <a name="sample-rest-request---client-certificate"></a>Örnek REST isteği - istemci sertifikası

```json
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled"
  }
}
```

### <a name="sample-rest-response---client-certificate"></a>Örnek REST yanıtı - istemci sertifikası

```json
HTTP/1.1 200 OKCache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="basic"></a>Temel

### <a name="request-body---basic"></a>İstek gövdesi - temel

Kimlik doğrulaması kullanarak eklerken `Basic` model, bu ek öğeler istek gövdesinde belirtin.

| Öğe | Gerekli | Açıklama |
|---------|----------|-------------|
| **kimlik doğrulaması** (üst öğe) | Temel kimlik doğrulamasını kullanarak kimlik doğrulaması nesnesi | 
| **type** | Evet | Kimlik doğrulaması türü. Temel kimlik doğrulaması için değerdir `Basic`. | 
| **Kullanıcı adı** | Evet | Kimlik doğrulaması için kullanıcı adı | 
| **Parola** | Evet | Kimlik doğrulaması için parola |
|||| 

### <a name="response-body---basic"></a>Yanıt gövdesi - temel

Yanıt, kimlik doğrulama bilgilerini bir istek gönderildiğinde, bu kimlik doğrulama öğeleri içerir.

| Öğe | Açıklama | 
|---------|-------------|
| **kimlik doğrulaması** (üst öğe) | Temel kimlik doğrulamasını kullanarak kimlik doğrulaması nesnesi |
| **type** | Kimlik doğrulaması türü. Temel kimlik doğrulaması için değerdir `Basic`. |
| **Kullanıcı adı** | Kimliği doğrulanmış kullanıcı adı |
||| 

### <a name="sample-rest-request---basic"></a>Örnek REST isteği - temel

```json
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled"
  }
}
```

### <a name="sample-rest-response---basic"></a>Örnek REST yanıtı - temel

```json
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"Http"
      },
      "recurrence":{  
         "frequency":"Minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"Enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="active-directory-oauth"></a>Active Directory OAuth

### <a name="request-body---active-directory-oauth"></a>İstek gövdesi - Active Directory OAuth 

Kimlik doğrulaması kullanarak eklerken `ActiveDirectoryOAuth` model, bu ek öğeler istek gövdesinde belirtin.

| Öğe | Gerekli | Açıklama |
|---------|----------|-------------|
| **kimlik doğrulaması** (üst öğe) | Evet | ActiveDirectoryOAuth kimlik doğrulamasını kullanarak kimlik doğrulaması nesnesi |
| **type** | Evet | Kimlik doğrulaması türü. ActiveDirectoryOAuth kimlik doğrulaması için değerdir `ActiveDirectoryOAuth`. |
| **Kiracı** | Evet | Azure AD kiracısı için Kiracı tanımlayıcısı. Azure AD kiracınız için Kiracı tanımlayıcısı için çalıştırın `Get-AzureAccount` Azure PowerShell'de. |
| **Hedef kitle** | Evet | Bu değeri şuna ayarlı `https://management.core.windows.net/`. | 
| **clientId** | Evet | Azure AD uygulamasının istemci tanımlayıcısı | 
| **Gizli anahtarı** | Evet | Belirteç istediği istemci gizli anahtarı | 
|||| 

### <a name="response-body---active-directory-oauth"></a>Yanıt gövdesi - Active Directory OAuth

Yanıt, kimlik doğrulama bilgilerini bir istek gönderildiğinde, bu kimlik doğrulama öğeleri içerir.

| Öğe | Açıklama |
|---------|-------------|
| **kimlik doğrulaması** (üst öğe) | ActiveDirectoryOAuth kimlik doğrulamasını kullanarak kimlik doğrulaması nesnesi |
| **type** | Kimlik doğrulaması türü. ActiveDirectoryOAuth kimlik doğrulaması için değerdir `ActiveDirectoryOAuth`. | 
| **Kiracı** | Azure AD kiracısı için Kiracı tanımlayıcısı |
| **Hedef kitle** | Bu değeri şuna ayarlı `https://management.core.windows.net/`. |
| **clientId** | Azure AD uygulamasının istemci tanımlayıcısı |
||| 

### <a name="sample-rest-request---active-directory-oauth"></a>Örnek REST isteği - Active Directory OAuth

```json
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "Http"
    },
    "recurrence": {
      "frequency": "Minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "Enabled"
  }
}
```

### <a name="sample-rest-response---active-directory-oauth"></a>REST yanıtı - Active Directory OAuth örneği

```json
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{
   "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type": "Microsoft.Scheduler/jobCollections/jobs",
   "name": "southeastasiajc/httpjob",
   "properties": {
      "startTime": "2015-05-14T14:10:00Z",
      "action": {  
         "request": {
            "uri": "https://mywebserviceendpoint.com",
            "method": "GET",
            "headers": {  
               "x-ms-version": "2013-03-01"
            },
            "authentication": {  
               "tenant": "microsoft.onmicrosoft.com",
               "audience": "https://management.core.windows.net/",
               "clientId": "dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type": "ActiveDirectoryOAuth"
            }
         },
         "type": "Http"
      },
      "recurrence": {  
         "frequency": "minute",
         "endTime": "2016-04-10T08:00:00Z",
         "interval": 1
      },
      "state": "Enabled",
      "status": {  
         "lastExecutionTime": "2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime": "2016-03-16T19:11:00Z",
         "executionCount": 5,
         "failureCount": 5,
         "faultedCount": 1
      }
   }
}
```

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Scheduler nedir?](scheduler-intro.md)
* [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)
* [Azure Scheduler REST API](https://msdn.microsoft.com/library/mt629143)
* [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)
