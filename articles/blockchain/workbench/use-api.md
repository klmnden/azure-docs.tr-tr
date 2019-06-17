---
title: Azure Blockchain Workbench REST API'sini kullanma
description: Azure Blockchain Workbench REST API'sini kullanmaya yönelik senaryolar
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 04/15/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 7fa72ad62d7d11c795422a203d13a4dd45484c19
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60896061"
---
# <a name="using-the-azure-blockchain-workbench-rest-api"></a>Azure Blockchain Workbench REST API'sini kullanma

Azure Blockchain Workbench REST API'si geliştiricilere ve bilgi çalışanlarına blok zinciri uygulamalarına zengin tümleştirmeler oluşturmaları için bir yol sağlar. Bu belgede size, Workbench REST API'sinin önemli bazı yöntemlerinde yol gösterilir. Örneğin, bir özel blok zinciri istemcisi oluşturmak bir geliştirici istediğini varsayalım. Bu blok zinciri istemci görüntülemek ve bunların atanmış blok zinciri uygulamaları ile etkileşim kurmak oturum açmış olan kullanıcı sağlar. İstemci kullanıcıların anlaşma örneklerini görüntülemesine ve akıllı anlaşmalarla ilgili işlemler yapmasına olanak tanır. Workbench REST API, istemci aşağıdaki işlemleri yapmak için oturum açmış kullanıcının bağlamında kullanır:

* Uygulamaları listeleme
* Uygulama için iş akışlarını listeleme
* İş akışı için akıllı anlaşma örneklerini listeleme
* Anlaşma için kullanılabilir eylemleri listeleme
* Anlaşma için bir eylem yürütme

Senaryoda kullanılan örnek blok zinciri uygulamaları olabilir [Github'dan indirilen](https://github.com/Azure-Samples/blockchain).

## <a name="list-applications"></a>Uygulamaları listeleme

Blok zinciri istemciyi bir kullanıcı oturumu açtığı sonra kullanıcı için tüm Blockchain Workbench'i uygulamaları almak için ilk görev gelir. Bu senaryoda, kullanıcının iki uygulamaya erişimi vardır:

1. [Varlık aktarımı](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer/readme.md)
2. [Refrigerated ulaştırma](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/refrigerated-transportation/readme.md)

[Applications GET API'sini](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationsget) kullanın:

``` http
GET /api/v1/applications
Authorization : Bearer {access token}
```

Yanıt, bir kullanıcı erişimi Blockchain Workbench içinde olan tüm blok zinciri uygulamaları listeler. Blockchain Workbench'i yöneticiler her blok zinciri uygulaması alın. Workbench olmayan Yöneticiler, en az bir ilişkili uygulama rolü veya bir ilişkili akıllı sözleşme örneği rolüne sahip oldukları tüm blok zincirleri paylaşıldıkça alın.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/applications?skip=2",
    "applications": [
        {
            "id": 1,
            "name": "AssetTransfer",
            "description": "Allows transfer of assets between a buyer and a seller, with appraisal/inspection functionality",
            "displayName": "Asset Transfer",
            "createdByUserId": 1,
            "createdDtTm": "2018-04-28T05:59:14.4733333",
            "enabled": true,
            "applicationRoles": null
        },
        {
            "id": 2,
            "name": "RefrigeratedTransportation",
            "description": "Application to track end-to-end transportation of perishable goods.",
            "displayName": "Refrigerated Transportation",
            "createdByUserId": 7,
            "createdDtTm": "2018-04-28T18:25:38.71",
            "enabled": true,
            "applicationRoles": null
        }
    ]
}
```

## <a name="list-workflows-for-an-application"></a>Uygulama için iş akışlarını listeleme

Bir kullanıcı uygun blok zinciri uygulaması seçtikten sonra (gibi **varlık aktarım**), blok zinciri istemci belirli bir blok zinciri uygulaması tüm iş akışları alır. Ardından, iş akışının tüm akıllı anlaşma örneklerinin gösterilmesi için kullanıcılar uygun iş akışını seçer. Her blok zinciri uygulamasının bir veya birden çok iş akışı vardır ve her iş akışının sıfır veya akıllı anlaşma örnekleri bulunur. Yalnızca bir iş akışı olan bir blok zinciri istemci uygulaması, kullanıcıların uygun iş akışı seçmesine izin veren kullanıcı deneyimi akışı atlanıyor öneririz. Bu durumda, **varlık aktarım** sahip olarak da bilinir, yalnızca bir iş akışı **varlık aktarım**.

[Applications Workflows GET API'sini](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/workflowsget) kullanın:

``` http
GET /api/v1/applications/{applicationId}/workflows
Authorization: Bearer {access token}
```

Yanıtta kullanıcının Blockchain Workbench'te erişimi olan belirli blok blok zinciri uygulamasının tüm iş akışları listelenir. Blockchain Workbench'i yöneticiler her blok zinciri iş akışı alır. Workbench olmayan Yöneticiler, bunlar en az bir ilişkili uygulama rolüne sahip olmanız veya akıllı sözleşme örneği rolüyle ilişkilendirilmiş tüm iş akışları alın.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/applications/1/workflows?skip=1",
    "workflows": [
        {
            "id": 1,
            "name": "AssetTransfer",
            "description": "Handles the business logic for the asset transfer scenario",
            "displayName": "Asset Transfer",
            "applicationId": 1,
            "constructorId": 1,
            "startStateId": 1
        }
    ]
}
```

## <a name="list-smart-contract-instances-for-a-workflow"></a>İş akışı için akıllı anlaşma örneklerini listeleme

Geçerli iş akışı, bu durumda bir kullanıcı seçtikten sonra **varlık aktarım**, blok zinciri istemci belirtilen iş akışı için tüm akıllı sözleşme örneklerini alır. İş akışı için tüm akıllı sözleşme örnekleri göstermek için bu bilgileri kullanabilirsiniz. Veya gösterilen akıllı sözleşme örnekleri hiçbirine yakından kullanıcılara izin verebilirsiniz. Bu örnekte, kullanıcının bir eylem yapmak için akıllı anlaşma örneklerinden biriyle etkileşimli çalışmak istediğini düşünün.

[Contracts GET API'sini](/rest/api/azure-blockchain-workbench/contractsv2/contractsget) kullanın:

``` http
GET api/v1/contracts?workflowId={workflowId}
Authorization: Bearer {access token}
```

Yanıtta belirtilen iş akışının tüm akıllı anlaşma örnekleri listelenir. Workbench Yöneticiler tüm akıllı sözleşme örnekleri alın. Workbench olmayan Yöneticiler, bunlar en az bir ilişkili uygulama rolüne sahip olmanız veya akıllı sözleşme örneği rolüyle ilişkilendirilmiş her akıllı sözleşme örneği alın.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/contracts?skip=3&workflowId=1",
    "contracts": [
        {
            "id": 1,
            "provisioningStatus": 2,
            "connectionID": 1,
            "ledgerIdentifier": "0xbcb6127be062acd37818af290c0e43479a153a1c",
            "deployedByUserId": 1,
            "workflowId": 1,
            "contractCodeId": 1,
            "contractProperties": [
                {
                    "workflowPropertyId": 1,
                    "value": "0"
                },
                {
                    "workflowPropertyId": 2,
                    "value": "My first car"
                },
                {
                    "workflowPropertyId": 3,
                    "value": "54321"
                },
                {
                    "workflowPropertyId": 4,
                    "value": "0"
                },
                {
                    "workflowPropertyId": 5,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 6,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 7,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 8,
                    "value": "0xd882530eb3d6395e697508287900c7679dbe02d7"
                }
            ],
            "transactions": [
                {
                    "id": 1,
                    "connectionId": 1,
                    "transactionHash": "0xf3abb829884dc396e03ae9e115a770b230fcf41bb03d39457201449e077080f4",
                    "blockID": 241,
                    "from": "0xd882530eb3d6395e697508287900c7679dbe02d7",
                    "to": null,
                    "value": 0,
                    "isAppBuilderTx": true
                }
            ],
            "contractActions": [
                {
                    "id": 1,
                    "userId": 1,
                    "provisioningStatus": 2,
                    "timestamp": "2018-04-29T23:41:14.9333333",
                    "parameters": [
                        {
                            "name": "Description",
                            "value": "My first car"
                        },
                        {
                            "name": "Price",
                            "value": "54321"
                        }
                    ],
                    "workflowFunctionId": 1,
                    "transactionId": 1,
                    "workflowStateId": 1
                }
            ]
        }
    ]
}
```

## <a name="list-available-actions-for-a-contract"></a>Anlaşma için kullanılabilir eylemleri listeleme

Bir kullanıcı bir sözleşme yakından karar sonra blok zinciri istemci durumu sözleşmenin mevcut kullanıcı eylemleri ardından gösterebilirsiniz. Bu örnekte, kullanıcı oluşturduğu yeni akıllı anlaşmanın tüm kullanılabilir eylemlerine bakıyor:

* Değiştirin: Açıklama ve fiyat bir varlığın değiştirme izin verir.
* Sonlandırma: Kullanıcının varlık sözleşme bitiş izin verir.

[Contract Action GET API'sini](/rest/api/azure-blockchain-workbench/contractsv2/contractactionget) kullanın:

``` http
GET /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
```

Yanıtta belirtilen akıllı anlaşma örneğinin geçerli durumuna göre kullanıcının gerçekleştirebileceği tüm eylemler listelenir. Belirtilen akıllı anlaşma örneğinin geçerli durumuna göre ilişkili bir uygulama rolü olan veya akıllı anlaşma örneği rolüyle ilişkili olan kullanıcılar tüm uygun eylemleri alır.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/contracts/1/actions?skip=2",
    "workflowFunctions": [
        {
            "id": 2,
            "name": "Modify",
            "description": "Modify the description/price attributes of this asset transfer instance",
            "displayName": "Modify",
            "parameters": [
                {
                    "id": 1,
                    "name": "description",
                    "description": "The new description of the asset",
                    "displayName": "Description",
                    "type": {
                        "id": 2,
                        "name": "string",
                        "elementType": null,
                        "elementTypeId": 0
                    }
                },
                {
                    "id": 2,
                    "name": "price",
                    "description": "The new price of the asset",
                    "displayName": "Price",
                    "type": {
                        "id": 3,
                        "name": "money",
                        "elementType": null,
                        "elementTypeId": 0
                    }
                }
            ],
            "workflowId": 1
        },
        {
            "id": 3,
            "name": "Terminate",
            "description": "Used to cancel this particular instance of asset transfer",
            "displayName": "Terminate",
            "parameters": [],
            "workflowId": 1
        }
    ]
}
```

## <a name="execute-an-action-for-a-contract"></a>Anlaşma için bir eylem yürütme

Ardından kullanıcı belirtilen akıllı anlaşma örneği için bir eylem gerçekleştirmeye karar verebilir. Bu durumda, bir kullanıcı burada açıklaması ve fiyatı şu eylemi için bir varlık, değiştirmek istediğiniz senaryoyu düşünün:

* Açıklama: "Benim güncelleştirilmiş araba"
* Fiyat: 54321

[Contract Action POST API'sini](/rest/api/azure-blockchain-workbench/contractsv2/contractactionpost) kullanın:

``` http
POST /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
actionInformation: {
    "workflowFunctionId": 2,
    "workflowActionParameters": [
        {
            "name": "description",
            "value": "My updated car"
        },
        {
            "name": "price",
            "value": "54321"
        }
    ]
}
```

Kullanıcılar ancak belirtilen akıllı anlaşma örneğinin geçerli durumuna ve kullanıcının ilişkili uygulama rolüne veya akıllı anlaşma örneği rolüne göre eylem yürütebilir. Gönderi başarılı olursa, yanıt gövdesi olmayan bir HTTP 200 OK yanıtı döndürülür.

``` http
HTTP/1.1 200 OK
Content-type: application/json
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench REST API'si başvurusu](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)