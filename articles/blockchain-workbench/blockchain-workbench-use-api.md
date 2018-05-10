---
title: Azure Blockchain çalışma ekranı REST API kullanarak
description: Azure Blockchain çalışma ekranı REST API kullanımı için senaryolar
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/2/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 27ed94b3ce14c57e369b0c80d4c53b72a5ae40a8
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="using-the-azure-blockchain-workbench-rest-api"></a>Azure Blockchain çalışma ekranı REST API kullanarak 

Azure Blockchain çalışma ekranı REST API geliştiricilere ve bilgi işçilerini blockchain uygulamaları için zengin tümleştirmeler oluşturmak için bir yol sağlar. Bu belge, çalışma ekranı REST API çeşitli anahtar yöntemler açıklanmaktadır. Burada oturum açmış olan kullanıcıların görüntülemek ve atanan blockchain uygulamalarını ile etkileşime girmesine izin veren özel blockchain istemci oluşturmak için bir geliştirici istediği bir senaryo düşünün. İstemci kullanıcıların sözleşme örneklerini görüntülemek ve akıllı sözleşmelerinde eylemleri olanak tanır. İstemci, aşağıdakileri yapmak için oturum açmış kullanıcının bağlamında çalışma ekranı REST API kullanır:

* Uygulamaları listeleme
* Bir uygulama için liste iş akışları
* Bir iş akışı için akıllı sözleşme örneklerini listele
* Bir sözleşme listesi kullanılabilir eylemler
* Bir sözleşme için bir eylem yürütme

## <a name="list-applications"></a>Uygulamaları listeleme

Bir kullanıcı blockchain istemciyi imzaladığı sonra ilk kullanıcı için tüm Blockchain çalışma ekranı blockchain uygulamaları almak için bir görevdir. Bu senaryoda, kullanıcının iki uygulama erişimi:

1.  Varlık aktarımı
2.  Refrigerated taşıma

Kullanım [uygulamaları GET API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationsget):

``` http
GET /api/v1/applications 
Authorization : Bearer {access token}
```

Yanıt için bir kullanıcı erişimi Blockchain çalışma ekranı içinde olan tüm blockchain uygulamaları listeler. En az bir ilişkili uygulama rolü veya bir ilişkili akıllı sözleşme örneği rolü sahip oldukları tüm blockchains çalışma ekranı olmayan Yöneticiler elde ederken Blockchain çalışma ekranı yöneticileri tüm blockchain uygulamaları alın.

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

## <a name="list-workflows-for-an-application"></a>Bir uygulama için liste iş akışları

Geçerli blockchain uygulama, bu durumda, varlık aktarma, bir kullanıcı seçtikten sonra blockchain istemci belirli blockchain uygulamanın tüm iş akışları alır. Kullanıcılar daha sonra iş akışı için tüm akıllı sözleşme örnekleri gösterilen önce geçerli iş akışı seçebilirsiniz. Bir veya daha fazla iş akışı her blockchain uygulamanın vardır ve her bir iş akışı sahip sıfır veya sözleşme örnekleri akıllı. Blockchain istemci uygulamaları oluştururken, kullanıcıların blockchain uygulama için yalnızca bir iş akışı olduğunda uygun iş akışı seçmesine izin verme kullanıcı deneyimi akışı atlamak için önerilir. Bu durumda, varlık aktarım varlık aktarımı olarak da adlandırılan tek bir iş akışı vardır.

Kullanım [uygulamaları iş akışları GET API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/workflowsget):

``` http
GET /api/v1/applications/{applicationId}/workflows
Authorization: Bearer {access token}
```

Yanıt belirtilen blockchain uygulamasının kullanıcı erişimi Blockchain çalışma ekranı içinde olan tüm iş akışları listeler. Blockchain çalışma ekranı Yöneticiler tüm blockchain iş akışları, çalışma ekranı olmayan yöneticiler en az bir ilişkili uygulama rolü sahip oldukları tüm iş akışlarını elde ederken almak veya bir akıllı sözleşme örneği rolüyle ilişkilendirilmiş.

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

## <a name="list-smart-contract-instances-for-a-workflow"></a>Bir iş akışı için akıllı sözleşme örneklerini listele

Geçerli iş akışı, bu örneği varlık aktarma, bir kullanıcı seçtikten sonra blockchain istemci belirtilen iş akışı için tüm akıllı sözleşme örnekleri alın. İş akışı için tüm akıllı sözleşme örnekleri Göster ve derin Dalış kullanıcılara gösterilen akıllı sözleşme örnekleri hiçbirine izin vermek için bu bilgileri kullanın. Bu örnekte, bir kullanıcı bir eyleme geçmek için akıllı sözleşme örnekleri ile etkileşim istediğiniz göz önünde bulundurun.

Kullanım [sözleşmeler GET API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractsget):

``` http
GET api/v1/contracts?workflowId={workflowId}
Authorization: Bearer {access token}
```

Yanıt, belirtilen iş akışı tüm akıllı sözleşme örneklerini listeler. Çalışma ekranı yöneticileri tüm akıllı sözleşme örnekleri alın, çalışma ekranı olmayan Yöneticiler alma sırasında tüm en az bir ilişkili uygulama rolü sahip oldukları sözleşme örnekleri akıllı veya bir akıllı sözleşme örneği rolüyle ilişkilidir.

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

## <a name="list-available-actions-for-a-contract"></a>Bir sözleşme listesi kullanılabilir eylemler

Bir kullanıcı için derin Dalış sözleşmeleri birine karar vermiştir sonra blockchain istemci sözleşme durumunu verilen kullanıcı ardından tüm eylemleri gösterebilir. Bu örnekte, kullanıcı yeni bir akıllı sözleşmesi oluşturma için hiç kullanılabilir eylemler arıyor:

* Değiştirin: açıklama ve bir varlığı bedelinin değiştirmek kullanıcının sağlar.
* Sonlandırma: Varlık sözleşme sonlandırmak kullanıcının sağlar.

Kullanım [sözleşme eylem GET API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractactionget):

``` http
GET /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
```

Yanıt belirtilen akıllı sözleşme örneğinin geçerli durumunu verilen bir kullanıcı işlemleri yapabileceğiniz tüm eylemleri listeler. Kullanıcı ilişkili uygulama rolü veya belirtilen akıllı sözleşme örneğinin geçerli durumu için akıllı sözleşme örneği rolüyle ilişkili varsa kullanıcılar tüm uygulanabilir eylemleri alır.

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

## <a name="execute-an-action-for-a-contract"></a>Bir sözleşme için bir eylem yürütme

Bir kullanıcı daha sonra belirtilen akıllı sözleşme örneği için önlem karar verebilirsiniz. Bu durumda, burada bir kullanıcı açıklama ve bir varlık şu bedelinin değiştirmek istediğiniz senaryoyu göz önünde bulundurun:

* Açıklama: "Benim güncelleştirilmiş araba"
* Fiyat: 54321

Kullanım [sözleşme eylem POST API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractactionpost):

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

Kullanıcılar yalnızca belirtilen akıllı sözleşme örneği ve kullanıcının ilişkili uygulama rolü veya akıllı sözleşme örneği rolü geçerli durumunu verilen eylemini yürütmek kullanabilirsiniz. Post başarılı olursa, bir HTTP 200 Tamam yanıt hiç yanıt gövdesi ile döndürülür.

``` http
HTTP/1.1 200 OK
Content-type: application/json
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Blockchain çalışma ekranı REST API Başvurusu](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)