---
title: Azure Blockchain Workbench yapılandırma başvurusu
description: Azure Blockchain Workbench uygulama yapılandırmasına genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 04/15/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
manager: femila
ms.openlocfilehash: 4d29d8e86a30f105c4aa50ec9615f8165fa238d3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60578991"
---
# <a name="azure-blockchain-workbench-configuration-reference"></a>Azure Blockchain Workbench yapılandırma başvurusu

Azure Blockchain Workbench çok taraflı iş akışlarını yapılandırma meta verilerini ve akıllı sözleşme kodu tarafından tanımlanan uygulamalardır. Yapılandırma meta verilerini, üst düzey iş akışları ve blok zinciri uygulaması etkileşim modelini tanımlar. Blok zinciri iş mantığına nitelikli akıllı anlaşmalar tanımlayın. Workbench, blok zinciri uygulaması kullanıcı deneyimleri oluşturmak için yapılandırma ve akıllı sözleşme kodu kullanır.

Her blok zinciri uygulaması için aşağıdaki bilgileri yapılandırma meta verilerini belirtir:

* Ad ve açıklama blok zinciri uygulaması
* Benzersiz rolleri hareket veya blok zinciri uygulaması içinde katılan kullanıcıları için
* Bir veya daha fazla iş akışları. Her bir iş akışı, iş mantığı akışını denetlemek için bir Durum Makinesi görev yapar. İş akışları, bağımsız veya birbiriyle etkileşim.

Tanımlanan her iş akışı aşağıdaki belirtir:

* Ad ve açıklama iş akışı
* İş akışı durumları.  Her bir durumu işlerinize'nın denetim akışında bir aşamayı ifade eder. 
* Sonraki duruma geçmek için Eylemler
* Her eylem başlatmak için izin verilen kullanıcı rolleri
* İş mantığı kod dosyalarında temsil nitelikli akıllı anlaşmalar

## <a name="application"></a>Uygulama

Bir blok zinciri uygulaması kimin işlem veya uygulama içinde katılmak yapılandırma meta verileri, iş akışları ve kullanıcı rolleri içerir.

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| ApplicationName | Benzersiz uygulama adı. Karşılık gelen akıllı sözleşmenin aynı kullanmalısınız **ApplicationName** uygulanabilir bir sözleşme sınıfı.  | Evet |
| displayName | Uygulama kolay görünen adı. | Evet |
| Açıklama | Uygulama açıklaması. | Hayır |
| ApplicationRoles | Koleksiyonu [ApplicationRoles](#application-roles). İşlem veya uygulama içinde katılmak kullanıcı rolleri.  | Evet |
| İş Akışları | Koleksiyonu [iş akışları](#workflows). Her bir iş akışı, iş mantığı akışını denetlemek için bir Durum Makinesi görev yapar. | Evet |

Bir örnek için bkz. [yapılandırma dosyası örneği](#configuration-file-example).

## <a name="workflows"></a>İş Akışları

Bir uygulamanın iş mantığı, burada bir durumdan diğerine taşımak için iş mantığı akışını bir eylemde neden olan bir Durum makinesi olarak modellenebilir. Bir iş akışı, bu tür durumları ve Eylemler oluşan bir koleksiyondur. Her bir iş akışı kodu dosyalarında iş mantığı temsil eden bir veya daha fazla akıllı sözleşmelerinizin oluşur. Bir iş akışı örneği bir yürütülebilir sözleşmedir.

| Alan | Açıklama | Gerekli | En büyük uzunluk |
|-------|-------------|:--------:|-----------:|
| Ad | Benzersiz iş akışının adı. Karşılık gelen akıllı sözleşmenin aynı kullanmalısınız **adı** uygulanabilir bir sözleşme sınıfı. | Evet | 50 |
| displayName | İş akışı kolay görünen adı. | Evet | 255 |
| Açıklama | İş akışı tanımı. | Hayır | 255 |
| Başlatıcılar | Koleksiyonu [ApplicationRoles](#application-roles). İş akışında sözleşmeleri oluşturmak için yetkili kullanıcılara atanan roller. | Evet | |
| StartState | İş akışı yapının başlangıç durumunun adı. | Evet | |
| Özellikler | Koleksiyonu [tanımlayıcıları](#identifiers). Araç zinciri kapalı veya bir kullanıcı görselleştirilen okunabilir temsil veri karşılaşırsınız. | Evet | |
| Oluşturucusu | İş akışı örneği oluşturmak için giriş parametrelerini tanımlar. | Evet | |
| İşlevler | Bir koleksiyonu [işlevleri](#functions) akışında çalıştırılabilir. | Evet | |
| Durumlar | İş akışı koleksiyonunu [durumları](#states). | Evet | |

Bir örnek için bkz. [yapılandırma dosyası örneği](#configuration-file-example).

## <a name="type"></a>Tür

Desteklenen veri türleri.

| Tür | Açıklama |
|-------|-------------|
| Adresi  | Blok zinciri adres türü gibi *sözleşmeleri* veya *kullanıcılar*. |
| array    | Tek düzey dizi türü tamsayı, bool, para ve zaman. Diziler, statik veya dinamik olabilir. Kullanım **ElementType** dizinin içindeki öğelerin veri türü belirtmek için. Bkz: [örnek yapılandırma](#example-configuration-of-type-array). |
| bool     | Boole veri türü. |
| Sözleşme | Sözleşme türü adresi. |
| Sabit listesi     | Adlandırılmış değerler numaralandırılmış kümesi. Enum türü kullanırken, ayrıca EnumValues listesini belirtin. Her değer 255 karakterle sınırlıdır. Geçerli değer üst karakterler ve küçük harfler (A-Z, a-z) ve sayılar (0-9). Bkz: [örnek yapılandırma ve kullanım Solidity](#example-configuration-of-type-enum). |
| int      | Tamsayı veri türü. |
| money    | Para veri türü. |
| state    | İş akışı durumu. |
| string  | Dize veri türü. 4000 karakter üst sınırı. Bkz: [örnek yapılandırma](#example-configuration-of-type-string). |
| kullanıcı     | Türü kullanıcı adresi. |
| time     | Saat veri türü. |
|`[ Application Role Name ]`| Uygulama rolünde belirtilen herhangi bir ad. Rol türü kullanıcıların sınırlar. |

### <a name="example-configuration-of-type-array"></a>Dizi türünde bir örnek yapılandırma

```json
{
  "Name": "Quotes",
  "Description": "Market quotes",
  "DisplayName": "Quotes",
  "Type": {
    "Name": "array",
    "ElementType": {
        "Name": "int"
    }
  }
}
```

#### <a name="using-a-property-of-type-array"></a>Dizi türünde bir özelliğini kullanma

Bir özelliğin yapılandırma türü dizi olarak tanımlarsanız, dizi türünün ortak özelliği Solidity döndürmek için bir açık get işlevi eklemeniz gerekir. Örneğin:

```
function GetQuotes() public constant returns (int[]) {
     return Quotes;
}
```

### <a name="example-configuration-of-type-string"></a>Dize türündeki örnek yapılandırma

``` json
{
  "Name": "description",
  "Description": "Descriptive text",
  "DisplayName": "Description",
  "Type": {
    "Name": "string"
  }
}
```

### <a name="example-configuration-of-type-enum"></a>Örnek yapılandırma enum türü

``` json
{
  "Name": "PropertyType",
  "DisplayName": "Property Type",
  "Description": "The type of the property",
  "Type": {
    "Name": "enum",
    "EnumValues": ["House", "Townhouse", "Condo", "Land"]
  }
}
```

#### <a name="using-enumeration-type-in-solidity"></a>Solidity içinde numaralandırma türü kullanma

Enum yapılandırmasında tanımlandıktan sonra Numaralandırma türleri Solidity kullanabilirsiniz. Örneğin, PropertyTypeEnum adlı bir enum tanımlayabilirsiniz.

```
enum PropertyTypeEnum {House, Townhouse, Condo, Land} PropertyTypeEnum public PropertyType; 
```

Dize listesi, yapılandırma ve Blockchain Workbench'i bildirimlerinde geçerli ve tutarlı olması için akıllı sözleşmesi arasındaki ile eşleşmesi gerekiyor.

Atama örnek:

```
PropertyType = PropertyTypeEnum.Townhouse;
```

İşlev parametresi örneği: 

``` 
function AssetTransfer(string description, uint256 price, PropertyTypeEnum propertyType) public
{
    InstanceOwner = msg.sender;
    AskingPrice = price;
    Description = description;
    PropertyType = propertyType;
    State = StateType.Active;
    ContractCreated();
}

```

## <a name="constructor"></a>Oluşturucusu

Bir iş akışı örneği için giriş parametrelerini tanımlar.

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| Parametreler | Koleksiyonu [tanımlayıcıları](#identifiers) akıllı bir sözleşme başlatması gerekli. | Evet |

### <a name="constructor-example"></a>Örnek oluşturucusu

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

İş akışını yürütülebilecek işlevleri tanımlar.

| Alan | Açıklama | Gerekli | En büyük uzunluk |
|-------|-------------|:--------:|-----------:|
| Ad | İşlev benzersiz adı. Karşılık gelen akıllı sözleşmenin aynı kullanmalısınız **adı** geçerli işlev. | Evet | 50 |
| displayName | İşlev kolay görünen adı. | Evet | 255 |
| Açıklama | İşlev açıklaması | Hayır | 255 |
| Parametreler | Koleksiyonu [tanımlayıcıları](#identifiers) karşılık gelen işlevin parametreleri. | Evet | |

### <a name="functions-example"></a>Örnek işlevleri

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

## <a name="states"></a>Durumlar

Bir iş akışındaki benzersiz durumlar koleksiyonudur. Her durum, iş mantığı'nın denetim akışı bir adımda yakalar. 

| Alan | Açıklama | Gerekli | En büyük uzunluk |
|-------|-------------|:--------:|-----------:|
| Ad | Durumun benzersiz adı. Karşılık gelen akıllı sözleşmenin aynı kullanmalısınız **adı** geçerli durumu için. | Evet | 50 |
| displayName | Durumun kolay görünen adı. | Evet | 255 |
| Açıklama | Durum açıklaması. | Hayır | 255 |
| PercentComplete | İş mantığı denetim akışı içinde ilerleme durumunu göstermek için Blockchain Workbench'i kullanıcı arabiriminde görüntülenen bir tamsayı değeri. | Evet | |
| Stil | Durumu'başarı veya başarısızlık durumu temsil edip etmediğini gösteren görsel ipucu. Geçerli iki değer vardır: `Success` veya `Failure`. | Evet | |
| Geçişleri | Kullanılabilir koleksiyonunu [geçişleri](#transitions) durumları sonraki kümesine geçerli durumu. | Hayır | |

### <a name="states-example"></a>Örnek durumları

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
      "Description": "Asset transfer has been canceled",
      "PercentComplete": 100,
      "Style": "Failure",
      "Transitions": []
    }
  ]
```

## <a name="transitions"></a>Geçişleri

Kullanılabilir eylemler için sonraki durum. Bir veya daha fazla kullanıcı rolleri, bir eylem burada başka bir durumda iş akışı durumuna geçiş her durumda, bir eylem gerçekleştirebilir. 

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| AllowedRoles | Geçişi başlatmak için izin verilen uygulamalar rollerinin listesi. Belirtilen rolün tüm kullanıcılar eylemi gerçekleştirmek mümkün olabilir. | Hayır |
| AllowedInstanceRoles | Katılma veya geçiş başlatmasına izin verilmiş akıllı sözleşmede belirtilen kullanıcı rolleri listesi. Örnek rollerinizin tanımlanmış **özellikleri** içinde iş akışları. AllowedInstanceRoles akıllı sözleşmenin bir örneğine katılan bir kullanıcıyı temsil eder. AllowedInstanceRoles bir eylemde bir sözleşme örneğinde bir kullanıcı rolüne sınırlandırılmasına olanak sağlar.  Örneğin, yalnızca Rol türü (sahibi) içindeki tüm kullanıcılar yerine AllowedRoles içinde rol belirtilmişse sonlandıramaz sözleşmenin (InstanceOwner) oluşturan kullanıcıya izin vermek isteyebilirsiniz. | Hayır |
| displayName | Geçiş kolay görünen adı. | Evet |
| Açıklama | Geçişin açıklaması. | Hayır |
| İşlev | Geçişi başlatmak için işlevin adı. | Evet |
| NextStates | Başarılı bir geçiş sonrasında olası sonraki durumları koleksiyonu. | Evet |

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

Uygulama rolleri hareket ya da uygulama içinde katılmak istiyorsanız kullanıcılara atanan roller kümesini tanımlar. Uygulama rolleri, Eylemler ve blok zinciri içinde katılım kısıtlamak için kullanılabilir uygulama ve karşılık gelen iş akışları. 

| Alan | Açıklama | Gerekli | En büyük uzunluk |
|-------|-------------|:--------:|-----------:|
| Ad | Uygulama rolünün benzersiz adı. Karşılık gelen akıllı sözleşmenin aynı kullanmalısınız **adı** uygun rolü için. Temel tür adları ayrılmıştır. Aynı ada sahip bir uygulama rolü adlandırılamıyor [türü](#type)| Evet | 50 |
| Açıklama | Uygulama rolü açıklaması. | Hayır | 255 |

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
## <a name="identifiers"></a>Tanımlayıcıları

Tanımlayıcılar, iş akışı özellikleri, kurucu ve işlev parametrelerini tanımlamak için kullanılan bilgileri koleksiyonunu temsil eder. 

| Alan | Açıklama | Gerekli | En büyük uzunluk |
|-------|-------------|:--------:|-----------:|
| Ad | Özellik veya parametre benzersiz adı. Karşılık gelen akıllı sözleşmenin aynı kullanmalısınız **adı** için geçerli bir özellik veya parametre. | Evet | 50 |
| displayName | Özellik veya parametre kolay görünen adı. | Evet | 255 |
| Açıklama | Özellik veya parametre açıklaması. | Hayır | 255 |

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

Varlık aktarımı alım ve satım bir denetçisi ve appraiser gerektiren yüksek değerli varlıklar için bir akıllı sözleşme senaryodur. Satıcılar, sıra varlıklarını bir varlık aktarımı akıllı sözleşme örnekleme tarafından listeleyebilirsiniz. Alıcılar akıllı sözleşmenin bir eylemde teklifler yapabilir ve diğer taraflara incelemek veya varlık eşyaların değerini belirle eylemleri gerçekleştirebilirsiniz. Varlık olarak işaretlendikten sonra her ikisi de inceledi ve sözleşmesini tamamlanmasını ayarlanmadan önce biçilen, satıcı ve alıcı satışı yeniden onaylar. Güncelleştirildiğinde işlemdeki her bir noktada, tüm katılımcılar durumuyla sözleşmenin görünürlüğe sahip. 

Kod dosyaları da dahil olmak üzere daha fazla bilgi için bkz. [Azure Blockchain Workbench için varlık aktarımı örneği](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer)

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
          "Description": "Asset transfer has been canceled",
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
> [Azure Blockchain Workbench REST API'si başvurusu](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)

