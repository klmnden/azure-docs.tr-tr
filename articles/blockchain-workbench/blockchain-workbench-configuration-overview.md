---
title: Azure Blockchain çalışma ekranı yapılandırma başvurusu
description: Azure Blockchain çalışma ekranı uygulama yapılandırmasına genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 3/20/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 5569a7608a61b4e79a03264e0ccf62682782264b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
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
| Görünen adı | Uygulama kolay görünen adı. | Evet |
| Açıklama | Uygulama açıklaması. | Hayır |
| ApplicationRoles | Koleksiyonu [ApplicationRoles](#application-roles). Kimin hareket veya uygulama içinde katılmak kullanıcı rolleri.  | Evet |
| İş akışları | Koleksiyonu [iş akışları](#workflows). Her bir iş akışı, iş mantığı akışını denetlemek için durum makinesinin davranır. | Evet |

Bir örnek için bkz: [yapılandırma dosyası örneği](#configuration-file-example).

## <a name="workflows"></a>İş akışları

Bir uygulamanın iş mantığı, burada bir durumdan diğerine taşımak için iş mantığı akışını bir eylemde neden olan bir Durum makinesi olarak modellenebilir. Bir iş akışı, bu tür durumları ve Eylemler koleksiyonudur. Her bir iş akışı kodda iş mantığı temsil eden bir veya daha fazla akıllı sözleşmeler oluşur. Bir iş akışı örneği bir yürütülebilir sözleşmedir.

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| Ad | Benzersiz iş akışının adı. Karşılık gelen akıllı sözleşme aynı kullanmalısınız **adı** geçerli sözleşme sınıfı için. | Evet |
| Görünen adı | İş akışı kolay görünen adı. | Evet |
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
| string   | Dize veri türü |
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
| Görünen adı | İşlev kolay görünen adı. | Evet |
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
| Görünen adı | Durum kolay görünen adı. | Evet |
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
| AllowedInstanceRoles | Katılma veya geçiş başlatmasına izin verilmiş akıllı sözleşmede belirtilen kullanıcı rolleri listesi. Örnek rolleriniz tanımlanmış **özellikleri** içinde iş akışları. Bu kullanıcılar kullanıcı katılan temsil veya rol türünün tüm kullanıcılar akıllı sözleşmede belirtilen. | Hayır |
| Görünen adı | Geçiş kolay görünen adı. | Evet |
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
| Görünen adı | Özellik veya parametre kolay görünen adı. | Evet |
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

Aşağıdaki örnek, bir istek ve bir Yanıtlayıcı isteğine yanıt olarak göndermek, bir istek sahibi gönderir temel istek-yanıt uygulama tanımlar.

``` json
{
  "ApplicationName": "HelloBlockchain",
  "DisplayName": "Hello, Blockchain!",
  "Description": "A simple application to send request and get response",
  "ApplicationRoles": [
    {
      "Name": "Requestor",
      "Description": "A person sending a request."
    },
    {
      "Name": "Responder",
      "Description": "A person responding to a request"
    }
  ],
  "Workflows": [
    {
      "Name": "RequestResponse",
      "DisplayName": "Request Response",
      "Description": "A simple workflow to send a request and receive a response.",
      "Initiators": [ "Requestor" ],
      "StartState": "Request",
      "Properties": [
        {
          "Name": "State",
          "DisplayName": "State",
          "Description": "Holds the state of the contract.",
          "Type": {
            "Name": "state"
          }
        },
        {
          "Name": "Requestor",
          "DisplayName": "Requestor",
          "Description": "A person sending a request.",
          "Type": {
            "Name": "Requestor"
          }
        },
        {
          "Name": "Responder",
          "DisplayName": "Responder",
          "Description": "A person sending a response.",
          "Type": {
            "Name": "Responder"
          }
        },
        {
          "Name": "RequestMessage",
          "DisplayName": "Request Message",
          "Description": "A request message.",
          "Type": {
            "Name": "string"
          }
        },
        {
          "Name": "ResponseMessage",
          "DisplayName": "Response Message",
          "Description": "A response message.",
          "Type": {
            "Name": "string"
          }
        }
      ],
      "Constructor": {
        "Parameters": [
          {
            "Name": "message",
            "Description": "...",
            "DisplayName": "Request Message",
            "Type": {
              "Name": "string"
            }
          }
        ]
      },
      "Functions": [
        {
          "Name": "SendRequest",
          "DisplayName": "Request",
          "Description": "...",
          "Parameters": [
            {
              "Name": "requestMessage",
              "Description": "...",
              "DisplayName": "Request Message",
              "Type": {
                "Name": "string"
              }
            }
          ]
        },
        {
          "Name": "SendResponse",
          "DisplayName": "Response",
          "Description": "...",
          "Parameters": [
            {
              "Name": "responseMessage",
              "Description": "...",
              "DisplayName": "Response Message",
              "Type": {
                "Name": "string"
              }
            }
          ]
        }
      ],
      "States": [
        {
          "Name": "Request",
          "DisplayName": "Request",
          "Description": "...",
          "PercentComplete": 50,
          "Value": 0,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": ["Responder"],
              "AllowedInstanceRoles": [],
              "Description": "...",
              "Function": "SendResponse",
              "NextStates": [ "Respond" ],
              "DisplayName": "Send Response"
            }
          ]
        },
        {
          "Name": "Respond",
          "DisplayName": "Respond",
          "Description": "...",
          "PercentComplete": 90,
          "Value": 1,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": ["Requestor"],
              "Description": "...",
              "Function": "SendRequest",
              "NextStates": [ "Request" ],
              "DisplayName": "Send Request"
            }
          ]
        }
      ]
    }
  ]
}
```
## <a name="next-steps"></a>Sonraki adımlar

[Azure Blockchain çalışma ekranı dağıtma](blockchain-workbench-deploy.md)

