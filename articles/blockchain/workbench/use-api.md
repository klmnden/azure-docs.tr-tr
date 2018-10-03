---
title: Azure Blockchain Workbench REST API'sini kullanma
description: Azure Blockchain Workbench REST API'sini kullanmaya yönelik senaryolar
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 2e9124213181fe32f3492e353b05ace89a9d6992
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48243393"
---
# <a name="using-the-azure-blockchain-workbench-rest-api"></a>Azure Blockchain Workbench REST API'sini kullanma 

Azure Blockchain Workbench REST API'si geliştiricilere ve bilgi çalışanlarına blok zinciri uygulamalarına zengin tümleştirmeler oluşturmaları için bir yol sağlar. Bu belgede size, Workbench REST API'sinin önemli bazı yöntemlerinde yol gösterilir. Geliştiricinin, oturum açmış kullanıcıların kendilerine atanmış blok zinciri uygulamalarını görüntülemesine ve bunlarla etkileşimli çalışmasına olanak tanıyan özel bir blok zinciri istemcisi oluşturmak istediği bir senaryo düşünün. İstemci kullanıcıların anlaşma örneklerini görüntülemesine ve akıllı anlaşmalarla ilgili işlemler yapmasına olanak tanır. İstemci aşağıdakileri yapmak için Workbench REST API'sini oturum açmış kullanıcı bağlamında kullanır:

* Uygulamaları listeleme
* Uygulama için iş akışlarını listeleme
* İş akışı için akıllı anlaşma örneklerini listeleme
* Anlaşma için kullanılabilir eylemleri listeleme
* Anlaşma için bir eylem yürütme

Senaryoda kullanılan örnek blok zinciri uygulamaları olabilir [Github'dan indirilen](https://github.com/Azure-Samples/blockchain). 

## <a name="list-applications"></a>Uygulamaları listeleme

Blok zinciri istemciyi bir kullanıcı oturumu açtığı sonra kullanıcı için tüm Blockchain Workbench'i uygulamaları almak için ilk görev gelir. Bu senaryoda, kullanıcının iki uygulamaya erişimi vardır:

1.  [Varlık aktarımı](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer/readme.md)
2.  [Refrigerated ulaştırma](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/refrigerated-transportation/readme.md)

[Applications GET API'sini](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationsget) kullanın:

``` http
GET /api/v1/applications 
Authorization : Bearer {access token}
```

Yanıt, bir kullanıcı erişimi Blockchain Workbench içinde olan tüm blok zinciri uygulamaları listeler. Blockchain Workbench yöneticileri tüm blok zinciri uygulamalarını alırken, Workbench yöneticisi olmayanlar en az bir ilişkili uygulama rolüne veya ilişkili akıllı anlaşma örneği rolüne sahip oldukları tüm blok zincirlerini alırlar.

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

Bir kullanıcı bu durumda, uygun blok zinciri uygulaması seçtikten sonra **varlık aktarım**, blok zinciri istemci belirli bir blok zinciri uygulaması tüm iş akışları alır. Ardından, iş akışının tüm akıllı anlaşma örneklerinin gösterilmesi için kullanıcılar uygun iş akışını seçer. Her blok zinciri uygulamasının bir veya birden çok iş akışı vardır ve her iş akışının sıfır veya akıllı anlaşma örnekleri bulunur. Blok zinciri istemci uygulamaları oluştururken, blok zinciri uygulamasının tek bir iş akışı olduğu durumlarda kullanıcıların uygun iş akışını seçmesine izin veren kullanıcı deneyimi akışının atlanması önerilir. Bu durumda, **varlık aktarım** sahip olarak da bilinir, yalnızca bir iş akışı **varlık aktarım**.

[Applications Workflows GET API'sini](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/workflowsget) kullanın:

``` http
GET /api/v1/applications/{applicationId}/workflows
Authorization: Bearer {access token}
```

Yanıtta kullanıcının Blockchain Workbench'te erişimi olan belirli blok blok zinciri uygulamasının tüm iş akışları listelenir. Blockchain Workbench yöneticileri tüm blok zinciri iş akışlarını alırken, Workbench yöneticisi olmayanlar en az bir ilişkili uygulama rolüne sahip oldukları veya akıllı anlaşma örneği rolüyle ilişkili oldukları tüm iş akışlarını alırlar.

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

Geçerli iş akışı, bu durumda bir kullanıcı seçtikten sonra **varlık aktarım**, blok zinciri istemci belirtilen iş akışı için tüm akıllı sözleşme örneklerini alır. Bu bilgiyi kullanarak iş akışının tüm akıllı anlaşma örneklerini gösterebilir ve kullanıcıların gösterilen tüm akıllı anlaşma örneklerinde ayrıntıya gitmesine olanak tanıyabilirsiniz. Bu örnekte, kullanıcının bir eylem yapmak için akıllı anlaşma örneklerinden biriyle etkileşimli çalışmak istediğini düşünün.

[Contracts GET API'sini](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractsget) kullanın:

``` http
GET api/v1/contracts?workflowId={workflowId}
Authorization: Bearer {access token}
```

Yanıtta belirtilen iş akışının tüm akıllı anlaşma örnekleri listelenir. Workbench yöneticileri tüm akıllı anlaşma örneklerini alırken, Workbench yöneticisi olmayanlar en az bir ilişkili uygulama rolüne sahip oldukları veya akıllı anlaşma örneği rolüyle ilişkili oldukları tüm akıllı anlaşma örneklerini alırlar.

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

Kullanıcı anlaşmaların birinde ayrıntıya gitmeye karar verdiğinde, blok zinciri istemcisi kullanıcıya anlaşmanın durumuna göre tüm kullanılabilir eylemleri gösterebilir. Bu örnekte, kullanıcı oluşturduğu yeni akıllı anlaşmanın tüm kullanılabilir eylemlerine bakıyor:

* Modify: Kullanıcının, bir varlığın açıklamasını ve fiyatını değiştirmesine olanak tanır.
* Terminate: Kullanıcının, varlığın anlaşmasını sonlandırmasına olanak tanır.

[Contract Action GET API'sini](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractactionget) kullanın:

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

Ardından kullanıcı belirtilen akıllı anlaşma örneği için bir eylem gerçekleştirmeye karar verebilir. Bu örnekte, kullanıcının varlığın açıklamasını ve fiyatını şu şekilde değiştirmek istediği bir senaryo düşünün:

* Açıklama: "My updated car"
* Fiyat: 54321

[Contract Action POST API'sini](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractactionpost) kullanın:

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