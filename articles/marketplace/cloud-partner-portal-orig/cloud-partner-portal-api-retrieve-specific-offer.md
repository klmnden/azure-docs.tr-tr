---
title: Belirli teklif API alma | Microsoft Docs
description: API yayımcı ad alanı içinde belirtilen teklif alır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 9484cf0f549db94be8f1ac2363addca952a3cff3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61096072"
---
<a name="retrieve-a-specific-offer"></a>Belirli bir teklif alma
=========================

Belirtilen teklif yayımcı ad alanı içindeki alır.  

Teklif, belirli bir sürümünü alma veya teklif taslağı, görünümü veya üretim yuvalarını Al. Bir yuva belirtilmezse, varsayılan değer `draft`. Yayımlanan ya da önizlendiğinde değil bir teklif almaya çalışırken sonuçlanır bir `404 Not Found` hata.

> [!WARNING]
> Gizli değerleri gizli tür alanları için bu API tarafından alınmayacak.

``` http
    GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>?api-version=2017-10-31

    GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/versions/<version>?api-version=2017-10-31

    GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/slot/<slotId>?api-version=2017-10-31

```


<a name="uri-parameters"></a>URI parametreleri
--------------


| **Ad**    | **Açıklama**                                                                          | **Veri türü** |
|-------------|------------------------------------------------------------------------------------------|---------------|
| publisherId | Publisherıd. Örneğin, Contoso                                                        | String        |
| offerId     | Teklifin benzersiz olarak tanımlayan GUID.                                                 | String        |
| version     | Teklif alınmasını sürümü. Varsayılan olarak, en son teklif sürümü alınır. | Tamsayı       |
| slotId      | Teklif alınmasına izin olduğu yuvası aşağıdakilerden biri olabilir:      <br/>  - `Draft` (varsayılan), şu anda taslak teklif sürümü alır.  <br/>  -  `Preview` şu anda önizlemede teklif sürümünü alır.     <br/>  -  `Production` şu anda üretimde teklif sürümünü alır.          |      Sabit listesi |
| API sürümü | API'sının en son sürümü                                                                    | Tarih          |
|  |  |  |


<a name="header"></a>Üst bilgi
------

|  **Ad**          |   **Değer**            |
|  ---------------   |  --------------        |
|  Content-Type      | `application/json`     |
|  Yetkilendirme     | `Bearer YOUR_TOKEN`    |
|  |  |


<a name="body-example"></a>Gövde örneği
------------

### <a name="response"></a>Yanıt

``` json
{
    "offerTypeId": "microsoft-azure-virtualmachines",
    "publisherId": "contoso",
    "status": "failed",
    "id": "059afc24-07de-4126-b004-4e42a51816fe",
    "version": 5,
    "definition": {
        "displayText": "Contoso Virtual Machine Offer",
        "offer": {
            "microsoft-azure-marketplace-testdrive.enabled": false,
            "microsoft-azure-marketplace-testdrive.videos": [],
            "microsoft-azure-marketplace.title": "Contoso App",
            "microsoft-azure-marketplace.summary": "Contoso App makes dev ops a breeze",
            "microsoft-azure-marketplace.longSummary": "Contoso App makes dev ops a breeze",
            "microsoft-azure-marketplace.description": "Contoso App makes dev ops a breeze",
            "microsoft-azure-marketplace.offerMarketingUrlIdentifier": "contosoapp",
            "microsoft-azure-marketplace.allowedSubscriptions": [
                "59160c40-2e25-4dcf-a2fd-6514cb08bf08"
            ],
            "microsoft-azure-marketplace.usefulLinks": [
            {
                "linkTitle": "Contoso App for Azure",
                "linkUrl": "https://azuremarketplace.microsoft.com"
            }
            ],
            "microsoft-azure-marketplace.categories": [
                "devService",
                "networking",
                "database",
                "cache",
                "security"
            ],
            "microsoft-azure-marketplace.smallLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/6218c455-9cbc-450c-9920-f2e7a69ee132.png?sv=2014-02-14&sr=b&sig=6O8MM9dgiJ48VK0MwddkyVbprRAnBszyhVkVHGShhkI%3D&se=2019-03-28T19%3A46%3A50Z&sp=r",
            "microsoft-azure-marketplace.mediumLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/557e714b-2f31-4e12-b0cc-e48dd840edf4.png?sv=2014-02-14&sr=b&sig=NwL67NTQf9Gc9VScmZehtbHXpYmxhwZc2foy3o4xavs%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
            "microsoft-azure-marketplace.largeLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/142485da-784c-44cb-9523-d4f396446258.png?sv=2014-02-14&sr=b&sig=xaMxhwx%2FlKYfz33mJGIg8UBdVpsOwVvqhjTJ883o0iY%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
            "microsoft-azure-marketplace.wideLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/48af9013-1df7-4c94-8da8-4626e5039ce0.png?sv=2014-02-14&sr=b&sig=%2BnN7f2tprkrqb45ID6JlT01zXcy1PMTkWXtLKD6nfoE%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
            "microsoft-azure-marketplace.screenshots": [],
            "microsoft-azure-marketplace.videos": [],
            "microsoft-azure-marketplace.leadDestination": "None",
            "microsoft-azure-marketplace.tableLeadConfiguration": {},
            "microsoft-azure-marketplace.blobLeadConfiguration": {},
            "microsoft-azure-marketplace.salesForceLeadConfiguration": {},
            "microsoft-azure-marketplace.crmLeadConfiguration": {},
            "microsoft-azure-marketplace.httpsEndpointLeadConfiguration": {},
            "microsoft-azure-marketplace.marketoLeadConfiguration": {},
            "microsoft-azure-marketplace.privacyURL": "https://azuremarketplace.microsoft.com",
            "microsoft-azure-marketplace.termsOfUse": "Terms of use",
            "microsoft-azure-marketplace.engineeringContactName": "Jon Doe",
            "microsoft-azure-marketplace.engineeringContactEmail": "jondoe@outlook.com",
            "microsoft-azure-marketplace.engineeringContactPhone": "555-555-5555",
            "microsoft-azure-marketplace.supportContactName": "Jon Doe",
            "microsoft-azure-marketplace.supportContactEmail": "jondoe@outlook.com",
            "microsoft-azure-marketplace.supportContactPhone": "555-555-5555",
            "microsoft-azure-marketplace.publicAzureSupportUrl": "",
            "microsoft-azure-marketplace.fairfaxSupportUrl": ""
            },
            "plans": [
            {
                "planId": "contososkuidentifier",
                "microsoft-azure-virtualmachines.skuTitle": "Contoso App",
                "microsoft-azure-virtualmachines.skuSummary": "Contoso App makes dev ops a breeze.",
                "microsoft-azure-virtualmachines.skuDescription": "This is a description for the Contoso App that makes dev ops a breeze.",
                "microsoft-azure-virtualmachines.hideSKUForSolutionTemplate": false,
                "microsoft-azure-virtualmachines.cloudAvailability": [
                    "PublicAzure"
                ],
                "microsoft-azure-virtualmachines.certificationsFairfax": [],
                "virtualMachinePricing": {
                    "isByol": true,
                    "freeTrialDurationInMonths": 0
                },
                "microsoft-azure-virtualmachines.operatingSystemFamily": "Windows",
                "microsoft-azure-virtualmachines.windowsOSType": "Other",
                "microsoft-azure-virtualmachines.operationSystem": "Contoso App",
                "microsoft-azure-virtualmachines.recommendedVMSizes": [
                    "a0-basic",
                    "a0-standard",
                    "a1-basic",
                    "a1-standard",
                    "a2-basic",
                    "a2-standard"
                ],
                "microsoft-azure-virtualmachines.openPorts": [],
                "microsoft-azure-virtualmachines.vmImages": {
                    "1.0.1": {
                    "osVhdUrl": "http://contosoteststorage.blob.core.windows.net/test/contosoVM.vhd?sv=2014-02-14&sig=WlDo6Q4xwYH%2B5QEJbItPUVdgHhBcrVxPBmntZ2vU96w%3D&st=2016-06-25T18%3A30%3A00Z&se=2017-06-25T18%3A30%3A00Z&sp=rl",
                    "lunVhdDetails": []
                    }
                },
                "regions": [
                    "DZ",
                    "AR"
                ]
            }
            ]
        },
        "changedTime": "2017-06-07T06:15:39.7349221Z"
    }
}
```


### <a name="response-body-properties"></a>Yanıt gövdesi özellikleri

|  **Ad**       |   **Açıklama**                                                                                                               |
|  -------------  |   -----------------------------------------------------------------------------------------------------                         |
|  offerTypeId    | Teklif türünü tanımlar                                                                                                    |
|  publisherId    | Benzersiz yayımcı tanımlayıcısı                                                                                              |
|  durum         | Teklif durumu. Olası değerler listesi için bkz: [teklif durumu](#offer-status) aşağıda.                                  |
|  Kimlik             | Teklifin benzersiz olarak tanımlayan GUID                                                                                         |
|  version        | Teklifin geçerli sürümü. İstemci tarafından version özelliği değiştirilemez. Bu, her yayımladıktan sonra artırılır.    |
|  tanım     | Gerçek iş yükü tanımı                                                                                               |
|  changedTime    | Teklif son değiştirildiği UTC tarih/saat                                                                                   |
|  |  |


### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod**  | **Açıklama**                                                                                                                 |
|  ------   | ------------------------------------------------------------------------------------------------------------------------------- |
|  200      | `OK` -İstek başarıyla işlendi ve yayımcı altındaki tüm teklifleri istemciye döndürülmedi.               |
|  400      | `Bad/Malformed request` -Hata yanıt gövdesi, daha fazla bilgi içeriyor olabilir.                                                 |
|  403      | `Forbidden` -İstemcisi belirtilen ad alanı için erişime sahip değil.                                                        |
|  404      | `Not found` -Belirtilen varlık yok. İstemci (belirtilmişse) Publisherıd, OfferId ve sürümünü denetlemelisiniz.      |
|  |  |


### <a name="offer-status"></a>Teklif durumu

|  **Ad**                   |   **Açıklama**                             |
| --------------------------- |  -------------------------------------------- |
|  NeverPublished             | Teklif, hiç yayımlanmadı.               |
|  NotStarted                 | Teklif, yeni ancak başlatılmadı.              |
|  WaitingForPublisherReview  | Teklif, yayımcı onay bekliyor.      |
|  Çalışıyor                    | Teklif gönderimi işleniyor.          |
|  Başarılı oldu                  | Teklif gönderme işlemi tamamlandı.    |
|  İptal edildi                   | Teklif gönderim iptal edildi.                |
|  Başarısız                     | Teklif gönderme başarısız oldu.                      |
|  |  |
