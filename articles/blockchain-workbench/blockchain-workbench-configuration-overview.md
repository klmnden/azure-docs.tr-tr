---
title: Azure Blockchain çalışma ekranı yapılandırma başvurusu
description: Azure Blockchain çalışma ekranı uygulama yapılandırmasına genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/16/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 178c2c1d4f727241338d6d933cd5eecbbffe65bb
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="azure-blockchain-workbench-configuration-reference"></a>Azure Blockchain çalışma ekranı yapılandırma başvurusu

 Azure Blockchain çalışma ekranı uygulamalarını yapılandırma meta verilerini ve akıllı sözleşme kodu tarafından tanımlanan çok kişili akışlarıdır. Yapılandırma meta verilerini üst düzey iş akışları ve blockchain uygulamasının etkileşim modelini tanımlar. Akıllı sözleşmeler blockchain uygulamanın iş mantığı tanımlar. Çalışma ekranı blockchain uygulama kullanıcı deneyimleri oluşturmak için yapılandırma ve akıllı sözleşme kodu kullanır.

Yapılandırma meta verilerini her blockchain uygulama için aşağıdaki bilgileri belirtir: 

* Ad ve açıklama blockchain uygulamasının
* Hareket veya blockchain uygulama içinde katılmak kullanıcılar için benzersiz rolleri
* Bir veya daha fazla iş akışı. Her bir iş akışı, iş mantığı akışını denetlemek için durum makinesinin davranır. İş akışları, bağımsız veya birbiriyle etkileşim.

Tanımlanan her iş akışı aşağıdaki belirtir:

* Ad ve açıklama iş akışı
* İş akışı durumları.  Her bir durumu, iş mantığı ait denetim akışında bir aşamadır. 
* Sonraki durum geçişi Eylemler
* Her eylem başlatmak için izin verilen kullanıcı rolleri
* Kod dosyaları iş mantığı temsil eden akıllı sözleşmeleri

## <a name="application"></a>Uygulama

Blockchain uygulama kimin hareket veya uygulama içinde katılmak yapılandırma meta verileri, iş akışları ve kullanıcı rolleri içerir.

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| ApplicationName | Benzersiz uygulama adı. Karşılık gelen akıllı sözleşme aynı kullanmalısınız **ApplicationName** geçerli sözleşme sınıfı için.  | Evet |
| DisplayName | Uygulama kolay görünen adı. | Evet |
| Açıklama | Uygulama açıklaması. | Hayır |
| ApplicationRoles | Koleksiyonu [ApplicationRoles](#application-roles). Kimin hareket veya uygulama içinde katılmak kullanıcı rolleri.  | Evet |
| İş akışları | Koleksiyonu [iş akışları](#workflows). Her bir iş akışı, iş mantığı akışını denetlemek için durum makinesinin davranır. | Evet |

Bir örnek için bkz: [yapılandırma dosyası örneği](#configuration-file-example).

## <a name="workflows"></a>İş akışları

Bir uygulamanın iş mantığı, burada bir durumdan diğerine taşımak için iş mantığı akışını bir eylemde neden olan bir Durum makinesi olarak modellenebilir. Bir iş akışı, bu tür durumları ve Eylemler koleksiyonudur. Her bir iş akışı kodda iş mantığı temsil eden bir veya daha fazla akıllı sözleşmeler oluşur. Bir iş akışı örneği bir yürütülebilir sözleşmedir.

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| Ad | Benzersiz iş akışının adı. Karşılık gelen akıllı sözleşme aynı kullanmalısınız **adı** geçerli sözleşme sınıfı için. | Evet |
| DisplayName | İş akışı kolay görünen adı. | Evet |
| Açıklama | İş akışı tanımı. | Hayır |
| Başlatıcıları | Koleksiyonu [ApplicationRoles](#application-roles). İş akışında sözleşmeleri oluşturmak için yetkili kullanıcılara atanan rolleri. | Evet |
| StartState | İş akışının İlk durumun adı. | Evet |
| Özellikler | Koleksiyonu [tanımlayıcıları](#identifiers). Zincir devre dışı veya bir kullanıcı görselleştirilmiş okunabilir temsil veri aracı deneyimi. | Evet |
| Oluşturucusu | İş akışı örneği oluşturmak için giriş parametrelerini tanımlar. | Evet |
| İşlevler | Bir koleksiyonu [işlevleri](#functions) akışında çalıştırılabilir. | Evet |
| durumları | İş akışı koleksiyonu [durumları](#states). | Evet |

Bir örnek için bkz: [yapılandırma dosyası örneği](#configuration-file-example).

## <a name="type"></a>Tür

Desteklenen veri türleri.

| Tür | Açıklama |
|-------|-------------|
| Adres  | Blockchain adres türü, gibi *sözleşmeleri* veya *kullanıcılar* |
| bool     | Boole veri türü |
| Sözleşme | Adres türü sözleşme |
| Int      | Integer veri türü |
| para    | Para veri türü |
| durum    | İş akışı durumu |
| dize   | Dize veri türü |
| kullanıcı     | Türü kullanıcının adresi |
| time     | Saat veri türü |
|`[ Application Role Name ]`| Uygulama rolünde belirtilen herhangi bir ad. Bu rol türü kullanıcıların sınırlar. |

## <a name="constructor"></a>Oluşturucusu

Bir iş akışı örneği için giriş parametrelerini tanımlar.

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| Parametreler | Koleksiyonu [tanımlayıcıları](#identifiers) akıllı sözleşme başlatmak için gerekli. | Evet |

### <a name="constructor-example"></a>Oluşturucu örneği

``` json
{
  "Parameters": [
    {
      "Name": "description",
      "Description": "The description of this asset",
      "DisplayName": "Description",
      "Type": {
        "Name": "string"
      }
    },
    {
      "Name": "price",
      "Description": "The price of this asset",
      "DisplayName": "Price",
      "Type": {
        "Name": "money"
      }
    }
  ]
}
```

## <a name="functions"></a>İşlevler

İş akışında yürütülebilecek işlevleri tanımlar.

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| Ad | İşlev benzersiz adı. Karşılık gelen akıllı sözleşme aynı kullanmalısınız **adı** geçerli işlevi için. | Evet |
| DisplayName | İşlev kolay görünen adı. | Evet |
| Açıklama | İşlev açıklaması | Hayır |
| Parametreler | Koleksiyonu [tanımlayıcıları](#identifiers) işlev parametreleri için karşılık gelen. | Evet |

### <a name="functions-example"></a>İşlevleri örneği

``` json
"Functions": [
  {
    "Name": "Modify",
    "DisplayName": "Modify",
    "Description": "Modify the description/price attributes of this asset transfer instance",
    "Parameters": [
      {
        "Name": "description",
        "Description": "The new description of the asset",
        "DisplayName": "Description",
        "Type": {
          "Name": "string"
        }
      },
      {
        "Name": "price",
        "Description": "The new price of the asset",
        "DisplayName": "Price",
        "Type": {
          "Name": "money"
        }
      }
    ]
  },
  {
    "Name": "Terminate",
    "DisplayName": "Terminate",
    "Description": "Used to cancel this particular instance of asset transfer",
    "Parameters": []
  }
]

```

## <a name="states"></a>durumları

Bir iş akışındaki benzersiz durumları koleksiyonu. Her bir durum, iş mantığı ait denetim akışı adımda yakalar. 

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| Ad | Durumun benzersiz adı. Karşılık gelen akıllı sözleşme aynı kullanmalısınız **adı** geçerli durumu için. | Evet |
| DisplayName | Durum kolay görünen adı. | Evet |
| Açıklama | Durum açıklaması. | Hayır |
| TamamlanmaYüzdesi | İş mantığı denetim akışı içinde ilerleme durumunu göstermek için Blockchain çalışma ekranı kullanıcı arabiriminde görüntülenen bir tamsayı değeri. | Evet |
| Stil | Durum başarı veya başarısızlık durumu temsil edip etmediğini gösteren görsel ipucu. Geçerli iki değer vardır: `Success` veya `Failure`. | Evet |
| Geçişleri | Kullanılabilir koleksiyonunu [geçişleri](#transitions) durumları sonraki kümesine geçerli durumu. | Hayır |

### <a name="states-example"></a>Durumlar örneği

``` json
"States": [
    {
      "Name": "Active",
      "DisplayName": "Active",
      "Description": "The initial state of the asset transfer workflow",
      "PercentComplete": 20,
      "Style": "Success",
      "Transitions": [
        {
          "AllowedRoles": [],
          "AllowedInstanceRoles": [ "InstanceOwner" ],
          "Description": "Cancels this instance of asset transfer",
          "Function": "Terminate",
          "NextStates": [ "Terminated" ],
          "DisplayName": "Terminate Offer"
        },
        {
          "AllowedRoles": [ "Buyer" ],
          "AllowedInstanceRoles": [],
          "Description": "Make an offer for this asset",
          "Function": "MakeOffer",
          "NextStates": [ "OfferPlaced" ],
          "DisplayName": "Make Offer"
        },
        {
          "AllowedRoles": [],
          "AllowedInstanceRoles": [ "InstanceOwner" ],
          "Description": "Modify attributes of this asset transfer instance",
          "Function": "Modify",
          "NextStates": [ "Active" ],
          "DisplayName": "Modify"
        }
      ]
    },
    {
      "Name": "Accepted",
      "DisplayName": "Accepted",
      "Description": "Asset transfer process is complete",
      "PercentComplete": 100,
      "Style": "Success",
      "Transitions": []
    },
    {
      "Name": "Terminated",
      "DisplayName": "Terminated",
      "Description": "Asset transfer has been cancelled",
      "PercentComplete": 100,
      "Style": "Failure",
      "Transitions": []
    }
  ]
```

## <a name="transitions"></a>Geçişleri

Sonraki durum eylemleri kullanılabilir. Bir veya daha fazla kullanıcı rolü, burada bir eylem iş akışı içinde başka bir duruma durumuna geçiş her durumda, bir eylem gerçekleştirebilir. 

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| AllowedRoles | Geçişi başlatmak için izin verilen uygulamaları rollerin listesini. Belirtilen rolün tüm kullanıcılar eylemi gerçekleştirmek mümkün olabilir. | Hayır |
| AllowedInstanceRoles | Katılma veya geçiş başlatmasına izin verilmiş akıllı sözleşmede belirtilen kullanıcı rolleri listesi. Örnek rolleriniz tanımlanmış **özellikleri** içinde iş akışları. AllowedInstanceRoles bir kullanıcı bir akıllı sözleşme örneğinde katılan temsil eder. AllowedInstanceRoles sınırlandırma bir eylemde bir sözleşme örneğinde bir kullanıcı rolüne verin.  Örneğin, yalnızca Rol türü (sahibi) tüm kullanıcılar yerine AllowedRoles içinde rol belirtilmişse sonlandırmak edebilmek için sözleşmeyi (InstanceOwner) oluşturan kullanıcının izin vermek isteyebilirsiniz. | Hayır |
| DisplayName | Geçiş kolay görünen adı. | Evet |
| Açıklama | Geçişin açıklaması. | Hayır |
| İşlev | Geçişi başlatmak için işlevin adı. | Evet |
| NextStates | Olası sonraki durumlarını koleksiyonu başarılı bir geçişten sonra. | Evet |

### <a name="transitions-example"></a>Geçişleri örneği

``` json
"Transitions": [
  {
    "AllowedRoles": [],
    "AllowedInstanceRoles": [ "InstanceOwner" ],
    "Description": "Cancels this instance of asset transfer",
    "Function": "Terminate",
    "NextStates": [ "Terminated" ],
    "DisplayName": "Terminate Offer"
  },
  {
    "AllowedRoles": [ "Buyer" ],
    "AllowedInstanceRoles": [],
    "Description": "Make an offer for this asset",
    "Function": "MakeOffer",
    "NextStates": [ "OfferPlaced" ],
    "DisplayName": "Make Offer"
  },
  {
    "AllowedRoles": [],
    "AllowedInstanceRoles": [ "InstanceOwner" ],
    "Description": "Modify attributes of this asset transfer instance",
    "Function": "Modify",
    "NextStates": [ "Active" ],
    "DisplayName": "Modify"
  }
]

```

## <a name="application-roles"></a>Uygulama rolleri

Uygulama rolleri hareket veya uygulama içinde katılmak isteyen kullanıcılara atanan roller kümesini tanımlar. Uygulama rolleri, eylemleri ve katılım blockchain içinde sınırlamak için kullanılabilir uygulama ve karşılık gelen iş akışları. 

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| Ad | Uygulama rolü benzersiz adı. Karşılık gelen akıllı sözleşme aynı kullanmalısınız **adı** geçerli rol için. Temel tür adları ayrılmıştır. Aynı ada sahip bir uygulama rolü adlandıramazsınız [türü](#type)| Evet |
| Açıklama | Uygulama rolü açıklaması. | Hayır |

### <a name="application-roles-example"></a>Uygulama rolleri örneği

``` json
"ApplicationRoles": [
  {
    "Name": "Appraiser",
    "Description": "User that signs off on the asset price"
  },
  {
    "Name": "Buyer",
    "Description": "User that places an offer on an asset"
  }
]
```
## <a name="identifiers"></a>Tanımlayıcılar

Tanımlayıcılar iş akışı özellikleri, oluşturucusu ve işlev parametrelerini tanımlamak için kullanılan bilgileri koleksiyonunu temsil eder. 

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| Ad | Özellik veya parametre benzersiz adı. Karşılık gelen akıllı sözleşme aynı kullanmalısınız **adı** uygulanabilir özellik veya parametre. | Evet |
| DisplayName | Özellik veya parametre kolay görünen adı. | Evet |
| Açıklama | Özellik veya parametre açıklaması. | Hayır |

### <a name="identifiers-example"></a>Tanımlayıcıları örneği

``` json
"Properties": [
  {
    "Name": "State",
    "DisplayName": "State",
    "Description": "Holds the state of the contract",
    "Type": {
      "Name": "state"
    }
  },
  {
    "Name": "Description",
    "DisplayName": "Description",
    "Description": "Describes the asset being sold",
    "Type": {
      "Name": "string"
    }
  }
]
```

## <a name="configuration-file-example"></a>Yapılandırma dosyası örneği

Varlık aktarımı, satın alma ve bir denetleyici ve appraiser gerektiren yüksek değerli varlıklara satış için bir akıllı sözleşme senaryodur. Satıcılar, bir varlık aktarımı akıllı sözleşme oluşturarak varlıklarına listeleyebilirsiniz. Alıcıların akıllı sözleşme bir eylemde teklifleri yapabilir ve diğer taraflar inceleyin veya varlık değer biç eylemleri gerçekleştirebilirsiniz. Varlık işaretlendikten sonra her ikisi de sahip denetlenir ve sözleşme tamamlamak için ayarlanmadan önce biçilen, satıcı ve alıcı satışı yeniden onaylar. Güncelleştirildiğinde işlemdeki her bir noktada, tüm katılımcılar sözleşme durumunu görünürlüğe sahip. 

Kod dosyaları dahil olmak üzere daha fazla bilgi için bkz: [varlık aktarımı örnek için Azure Blockchain çalışma ekranı](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer)

Aşağıdaki yapılandırma dosyası için varlık aktarımı örnektir:

``` json
{
  "ApplicationName": "AssetTransfer",
  "DisplayName": "Asset Transfer",
  "Description": "Allows transfer of assets between a buyer and a seller, with appraisal/inspection functionality",
  "ApplicationRoles": [
    {
      "Name": "Appraiser",
      "Description": "User that signs off on the asset price"
    },
    {
      "Name": "Buyer",
      "Description": "User that places an offer on an asset"
    },
    {
      "Name": "Inspector",
      "Description": "User that inspects the asset and signs off on inspection"
    },
    {
      "Name": "Owner",
      "Description": "User that signs off on the asset price"
    }
  ],
  "Workflows": [
    {
      "Name": "AssetTransfer",
      "DisplayName": "Asset Transfer",
      "Description": "Handles the business logic for the asset transfer scenario",
      "Initiators": [ "Owner" ],
      "StartState":  "Active",
      "Properties": [
        {
          "Name": "State",
          "DisplayName": "State",
          "Description": "Holds the state of the contract",
          "Type": {
            "Name": "state"
          }
        },
        {
          "Name": "Description",
          "DisplayName": "Description",
          "Description": "Describes the asset being sold",
          "Type": {
            "Name": "string"
          }
        },
        {
          "Name": "AskingPrice",
          "DisplayName": "Asking Price",
          "Description": "The asking price for the asset",
          "Type": {
            "Name": "money"
          }
        },
        {
          "Name": "OfferPrice",
          "DisplayName": "Offer Price",
          "Description": "The price being offered for the asset",
          "Type": {
            "Name": "money"
          }
        },
        {
          "Name": "InstanceAppraiser",
          "DisplayName": "Instance Appraiser",
          "Description": "The user that appraises the asset",
          "Type": {
            "Name": "Appraiser"
          }
        },
        {
          "Name": "InstanceBuyer",
          "DisplayName": "Instance Buyer",
          "Description": "The user that places an offer for this asset",
          "Type": {
            "Name": "Buyer"
          }
        },
        {
          "Name": "InstanceInspector",
          "DisplayName": "Instance Inspector",
          "Description": "The user that inspects this asset",
          "Type": {
            "Name": "Inspector"
          }
        },
        {
          "Name": "InstanceOwner",
          "DisplayName": "Instance Owner",
          "Description": "The seller of this particular asset",
          "Type": {
            "Name": "Owner"
          }
        }
      ],
      "Constructor": {
        "Parameters": [
          {
            "Name": "description",
            "Description": "The description of this asset",
            "DisplayName": "Description",
            "Type": {
              "Name": "string"
            }
          },
          {
            "Name": "price",
            "Description": "The price of this asset",
            "DisplayName": "Price",
            "Type": {
              "Name": "money"
            }
          }
        ]
      },
      "Functions": [
        {
          "Name": "Modify",
          "DisplayName": "Modify",
          "Description": "Modify the description/price attributes of this asset transfer instance",
          "Parameters": [
            {
              "Name": "description",
              "Description": "The new description of the asset",
              "DisplayName": "Description",
              "Type": {
                "Name": "string"
              }
            },
            {
              "Name": "price",
              "Description": "The new price of the asset",
              "DisplayName": "Price",
              "Type": {
                "Name": "money"
              }
            }
          ]
        },
        {
          "Name": "Terminate",
          "DisplayName": "Terminate",
          "Description": "Used to cancel this particular instance of asset transfer",
          "Parameters": []
        },
        {
          "Name": "MakeOffer",
          "DisplayName": "Make Offer",
          "Description": "Place an offer for this asset",
          "Parameters": [
            {
              "Name": "inspector",
              "Description": "Specify a user to inspect this asset",
              "DisplayName": "Inspector",
              "Type": {
                "Name": "Inspector"
              }
            },
            {
              "Name": "appraiser",
              "Description": "Specify a user to appraise this asset",
              "DisplayName": "Appraiser",
              "Type": {
                "Name": "Appraiser"
              }
            },
            {
              "Name": "offerPrice",
              "Description": "Specify your offer price for this asset",
              "DisplayName": "Offer Price",
              "Type": {
                "Name": "money"
              }
            }
          ]
        },
        {
          "Name": "Reject",
          "DisplayName": "Reject",
          "Description": "Reject the user's offer",
          "Parameters": []
        },
        {
          "Name": "AcceptOffer",
          "DisplayName": "Accept Offer",
          "Description": "Accept the user's offer",
          "Parameters": []
        },
        {
          "Name": "RescindOffer",
          "DisplayName": "Rescind Offer",
          "Description": "Rescind your placed offer",
          "Parameters": []
        },
        {
          "Name": "ModifyOffer",
          "DisplayName": "Modify Offer",
          "Description": "Modify the price of your placed offer",
          "Parameters": [
            {
              "Name": "offerPrice",
              "DisplayName": "Price",
              "Type": {
                "Name": "money"
              }
            }
          ]
        },
        {
          "Name": "Accept",
          "DisplayName": "Accept",
          "Description": "Accept the inspection/appraisal results",
          "Parameters": []
        },
        {
          "Name": "MarkInspected",
          "DisplayName": "Mark Inspected",
          "Description": "Mark the asset as inspected",
          "Parameters": []
        },
        {
          "Name": "MarkAppraised",
          "DisplayName": "Mark Appraised",
          "Description": "Mark the asset as appraised",
          "Parameters": []
        }
      ],
      "States": [
        {
          "Name": "Active",
          "DisplayName": "Active",
          "Description": "The initial state of the asset transfer workflow",
          "PercentComplete": 20,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancels this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate Offer"
            },
            {
              "AllowedRoles": [ "Buyer" ],
              "AllowedInstanceRoles": [],
              "Description": "Make an offer for this asset",
              "Function": "MakeOffer",
              "NextStates": [ "OfferPlaced" ],
              "DisplayName": "Make Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Modify attributes of this asset transfer instance",
              "Function": "Modify",
              "NextStates": [ "Active" ],
              "DisplayName": "Modify"
            }
          ]
        },
        {
          "Name": "OfferPlaced",
          "DisplayName": "Offer Placed",
          "Description": "Offer has been placed for the asset",
          "PercentComplete": 30,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Accept the proposed offer for the asset",
              "Function": "AcceptOffer",
              "NextStates": [ "PendingInspection" ],
              "DisplayName": "Accept Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the proposed offer for the asset",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you previously placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Modify the price that you specified for your offer",
              "Function": "ModifyOffer",
              "NextStates": [ "OfferPlaced" ],
              "DisplayName": "Modify Offer"
            }
          ]
        },
        {
          "Name": "PendingInspection",
          "DisplayName": "Pending Inspection",
          "Description": "Asset is pending inspection",
          "PercentComplete": 40,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the offer",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel the offer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceInspector" ],
              "Description": "Mark this asset as inspected",
              "Function": "MarkInspected",
              "NextStates": [ "Inspected" ],
              "DisplayName": "Mark Inspected"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceAppraiser" ],
              "Description": "Mark this asset as appraised",
              "Function": "MarkAppraised",
              "NextStates": [ "Appraised" ],
              "DisplayName": "Mark Appraised"
            }
          ]
        },
        {
          "Name": "Inspected",
          "DisplayName": "Inspected",
          "PercentComplete": 45,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the offer",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel the offer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceAppraiser" ],
              "Description": "Mark this asset as appraised",
              "Function": "MarkAppraised",
              "NextStates": [ "NotionalAcceptance" ],
              "DisplayName": "Mark Appraised"
            }
          ]
        },
        {
          "Name": "Appraised",
          "DisplayName": "Appraised",
          "Description": "Asset has been appraised, now awaiting inspection",
          "PercentComplete": 45,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the offer",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel the offer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceInspector" ],
              "Description": "Mark the asset as inspected",
              "Function": "MarkInspected",
              "NextStates": [ "NotionalAcceptance" ],
              "DisplayName": "Mark Inspected"
            }
          ]
        },
        {
          "Name": "NotionalAcceptance",
          "DisplayName": "Notional Acceptance",
          "Description": "Asset has been inspected and appraised, awaiting final sign-off from buyer and seller",
          "PercentComplete": 50,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "SellerAccepted" ],
              "DisplayName": "SellerAccept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the proposed offer for the asset",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "BuyerAccepted" ],
              "DisplayName": "BuyerAccept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            }
          ]
        },
        {
          "Name": "BuyerAccepted",
          "DisplayName": "Buyer Accepted",
          "Description": "Buyer has signed-off on inspection and appraisal",
          "PercentComplete": 75,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "SellerAccepted" ],
              "DisplayName": "Accept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the proposed offer for the asset",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            }
          ]
        },
        {
          "Name": "SellerAccepted",
          "DisplayName": "Seller Accepted",
          "Description": "Seller has signed-off on inspection and appraisal",
          "PercentComplete": 75,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "Accepted" ],
              "DisplayName": "Accept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            }
          ]
        },
        {
          "Name": "Accepted",
          "DisplayName": "Accepted",
          "Description": "Asset transfer process is complete",
          "PercentComplete": 100,
          "Style": "Success",
          "Transitions": []
        },
        {
          "Name": "Terminated",
          "DisplayName": "Terminated",
          "Description": "Asset transfer has been cancelled",
          "PercentComplete": 100,
          "Style": "Failure",
          "Transitions": []
        }
      ]
    }
  ]
}
```
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain çalışma ekranı REST API Başvurusu](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)

