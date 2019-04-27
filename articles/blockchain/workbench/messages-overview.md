---
title: Azure Blockchain Workbench iletileri tümleştirmesine genel bakış
description: Azure Blockchain Workbench uygulamasında mesajları genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 02/21/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
manager: femila
ms.openlocfilehash: 860c00b876427af7395e3c04e0626131c27aca67
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60896430"
---
# <a name="azure-blockchain-workbench-messaging-integration"></a>Azure Blockchain Workbench ile tümleştirme Mesajlaşma

REST API yanı sıra, Azure Blockchain Workbench de ileti tabanlı bir tümleştirme sağlar. Workbench, muhasebe merkezli olayları Azure Event Grid, veri alma veya bu olaylara göre eylemde bulunmak aşağı akış tüketiciler etkinleştirme aracılığıyla yayımlar. Güvenilir Mesajlaşma gerekli istemcilerinin Azure Blockchain Workbench iletileri bir Azure Service Bus uç noktası için de sunar.

## <a name="input-apis"></a>Giriş API'leri

İşlem kullanıcıları oluşturun, sözleşmeler oluşturun ve sözleşmeleri güncelleştirmek için dış sistemlerden başlatmak istiyorsanız, bir kayıt defteri üzerinde işlemler gerçekleştirmek için giriş API'leri Mesajlaşma'ı kullanabilirsiniz. Bkz: [tümleştirme örnekleri Mesajlaşma](https://aka.ms/blockchain-workbench-integration-sample) giriş API'leri gösteren bir örnek için.

Şu anda kullanılabilir giriş API'leri aşağıda verilmiştir.

### <a name="create-user"></a>Kullanıcı oluştur

Yeni bir kullanıcı oluşturur.

İstek, aşağıdaki alanları gerektirir:

| **Ad**             | **Açıklama**                                      |
|----------------------|------------------------------------------------------|
| requestId            | İstemci tarafından sağlanan GUID                                |
| FirstName            | Kullanıcı adı                              |
| Soyadı             | Kullanıcının soyadı                               |
| EmailAddress         | Kullanıcının e-posta adresi                           |
| externalId           | Kullanıcının Azure AD nesnesi kimliği                      |
| ConnectionID         | Blok zinciri bağlantı için benzersiz tanımlayıcı |
| messageSchemaVersion | Mesajlaşma şema sürümü                            |
| messageName          | **CreateUserRequest**                               |

Örnek:

``` json
{
    "requestId": "e2264523-6147-41fc-bbbb-edba8e44562d",
    "firstName": "Ali",
    "lastName": "Alio",
    "emailAddress": "aa@contoso.com",
    "externalId": "6a9b7f65-ffff-442f-b3b8-58a35abd1bcd",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateUserRequest"
}
```

Blockchain Workbench'i şu alanlara sahip bir yanıt döndürür:

| **Ad**              | **Açıklama**                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------|
| requestId             | İstemci tarafından sağlanan GUID |
| userId                | Oluşturulan kullanıcının kimliği |
| UserChainIdentifier   | Blok zinciri ağ üzerinde oluşturulan kullanıcı adresi. Ethereum, kullanıcının adresidir **zincir** adresi. |
| ConnectionID          | Blok zinciri bağlantı için benzersiz tanımlayıcı|
| messageSchemaVersion  | Mesajlaşma şema sürümü |
| messageName           | **CreateUserUpdate** |
| durum                | Kullanıcı oluşturma isteği durumu.  Başarılı, değer olup olmadığını **başarı**. Hata durumunda değerdir **hatası**.     |
| AdditionalInformation | Ek bilgi sağlanan temel durumu |

Örnek başarılı **oluşturacağı** Blockchain Workbench'i gelen yanıt:

``` json
{ 
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Success", 
    "additionalInformation": { } 
} 
```

İstek başarısız oldu, hata hakkındaki ayrıntılar ek bilgileri dahil edilir.

``` json
{
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": null, 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Failure", 
    "additionalInformation": { 
        "errorCode": 4000, 
        "errorMessage": "User cannot be provisioned on connection." 
    }
}
```

### <a name="create-contract"></a>Sözleşmesi oluşturma

Yeni bir sözleşmeyi oluşturur.

İstek, aşağıdaki alanları gerektirir:

| **Ad**             | **Açıklama**                                                                                                           |
|----------------------|---------------------------------------------------------------------------------------------------------------------------|
| requestId            | İstemci tarafından sağlanan GUID |
| UserChainIdentifier  | Blok zinciri ağ üzerinde oluşturulan kullanıcı adresi. Ethereum bu kullanıcının adresidir **zincirinde** adresi. |
| ApplicationName      | Uygulamanın adı |
| version              | Uygulamanın sürümü. Etkin bir uygulama birden çok sürümünü varsa gereklidir. Aksi takdirde sürüm isteğe bağlıdır. Uygulama sürümü hakkında daha fazla bilgi için bkz. [Azure Blockchain Workbench uygulama sürümü](version-app.md). |
| WorkflowName         | İş akışının adı |
| parametreler           | Sözleşme oluşturma için giriş parametreleri |
| ConnectionID         | Blok zinciri bağlantı için benzersiz tanımlayıcı |
| messageSchemaVersion | Mesajlaşma şema sürümü |
| messageName          | **CreateContractRequest** |

Örnek:

``` json
{ 
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211", 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "applicationName": "AssetTransfer",
    "version": "1.0",
    "workflowName": "AssetTransfer", 
    "parameters": [ 
        { 
            "name": "description", 
            "value": "a 1969 dodge charger" 
        }, 
        { 
            "name": "price", 
            "value": "12345" 
        } 
    ], 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateContractRequest" 
}
```

Blockchain Workbench'i şu alanlara sahip bir yanıt döndürür:

| **Ad**                 | **Açıklama**                                                                   |
|--------------------------|-----------------------------------------------------------------------------------|
| requestId                | İstemci tarafından sağlanan GUID                                                             |
| ContractId               | Azure Blockchain Workbench içinde sözleşme için benzersiz tanımlayıcı |
| ContractLedgerIdentifier | Genel muhasebe sözleşme adresi                                            |
| ConnectionID             | Blok zinciri bağlantı için benzersiz tanımlayıcı                               |
| messageSchemaVersion     | Mesajlaşma şema sürümü                                                         |
| messageName              | **CreateContractUpdate**                                                      |
| durum                   | Sözleşme oluşturma isteği durumu.  Olası değerler: **Gönderilen**, **kaydedilmiş**, **hatası**.  |
| AdditionalInformation    | Ek bilgi sağlanan temel durumu                              |

Gönderilen bir örneği **sözleşmesi oluşturma** Blockchain Workbench'i gelen yanıt:

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Submitted"
    "additionalInformation": { }
}
```

Örnek bir taahhüt **sözleşmesi oluşturma** Blockchain Workbench'i gelen yanıt:

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Committed",
    "additionalInformation": { }
}
```

İstek başarısız oldu, hata hakkındaki ayrıntılar ek bilgileri dahil edilir.

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": null,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Failure"
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract cannot be provisioned on connection."
    }
}
```

### <a name="create-contract-action"></a>Sözleşme Eylem oluştur

Yeni bir sözleşme eylem oluşturur.

İstek, aşağıdaki alanları gerektirir:

| **Ad**                 | **Açıklama**                                                                                                           |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------|
| requestId                | İstemci tarafından sağlanan GUID |
| UserChainIdentifier      | Blok zinciri ağ üzerinde oluşturulan kullanıcı adresi. Ethereum bu kullanıcının adresidir **zincirinde** adresi. |
| ContractLedgerIdentifier | Genel muhasebe sözleşme adresi |
| version                  | Uygulamanın sürümü. Etkin bir uygulama birden çok sürümünü varsa gereklidir. Aksi takdirde sürüm isteğe bağlıdır. Uygulama sürümü hakkında daha fazla bilgi için bkz. [Azure Blockchain Workbench uygulama sürümü](version-app.md). |
| WorkflowFunctionName     | İş akışı işlevinin adı |
| parametreler               | Sözleşme oluşturma için giriş parametreleri |
| ConnectionID             | Blok zinciri bağlantı için benzersiz tanımlayıcı |
| messageSchemaVersion     | Mesajlaşma şema sürümü |
| messageName              | **CreateContractActionRequest** |

Örnek:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398",
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "version": "1.0",
    "workflowFunctionName": "modify",
    "parameters": [
        {
            "name": "description",
            "value": "a 1969 dodge charger"
        },
        {
            "name": "price",
            "value": "12345"
        }
    ],
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionRequest"
}
```

Blockchain Workbench'i şu alanlara sahip bir yanıt döndürür:

| **Ad**              | **Açıklama**                                                                   |
|-----------------------|-----------------------------------------------------------------------------------|
| requestId             | İstemci tarafından sağlanan GUID|
| ContractId            | Azure Blockchain Workbench içinde sözleşme için benzersiz tanımlayıcı |
| ConnectionID          | Blok zinciri bağlantı için benzersiz tanımlayıcı |
| messageSchemaVersion  | Mesajlaşma şema sürümü |
| messageName           | **CreateContractActionUpdate** |
| durum                | Sözleşme eylem isteğinin durumu. Olası değerler: **Gönderilen**, **kaydedilmiş**, **hatası**.                         |
| AdditionalInformation | Ek bilgi sağlanan temel durumu |

Gönderilen bir örneği **sözleşme Eylem oluştur** Blockchain Workbench'i gelen yanıt:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Submitted",
    "additionalInformation": { }
}
```

Örnek bir taahhüt **sözleşme Eylem oluştur** Blockchain Workbench'i gelen yanıt:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Committed"
    "additionalInformation": { }
}
```

İstek başarısız oldu, hata hakkındaki ayrıntılar ek bilgileri dahil edilir.

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Failure"
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract action cannot be provisioned on connection."
    }
}
```

### <a name="input-api-error-codes-and-messages"></a>Giriş API'si hata kodları ve iletiler

**Hata kodu 4000: Hatalı istek hatası**
- Geçersiz ConnectionID
- CreateUserRequest seri kaldırma başarısız oldu
- CreateContractRequest seri kaldırma başarısız oldu
- CreateContractActionRequest seri kaldırma başarısız oldu
- {Uygulama adına göre tanımlanan} uygulaması yok
- Uygulama {uygulama adına göre tanımlanan} iş akışı yok.
- UserChainIdentifier yok
- {Muhasebe tanımlayıcısı tarafından tanımlanan} sözleşme yok.
- {Muhasebe tanımlayıcısı tarafından tanımlanan} sözleşme işlevi yok {iş akışı işlevi adı}
- UserChainIdentifier yok

**Hata kodu 4090: Çakışma hatası**
- Kullanıcı zaten var.
- Sözleşme zaten var.
- Sözleşme eylemi zaten var.

**Hata kodu 5000: İç sunucu hatası**
- Özel durum iletileri

## <a name="event-notifications"></a>Olay bildirimleri

Olay bildirimleri, kullanıcılar ve aşağı akış sistemlerine Blockchain Workbench'i ve bağlı olduğu blockchain ağ gerçekleşen olayları bildirmek için kullanılabilir. Olay bildirimleri doğrudan kod içinde kullanılan veya Logic Apps ve Flow gibi araçlarla aşağı akış sistemlerine veri akışı tetiklemek için kullanılır.

Bkz: [bildirim iletisi başvurusu](#notification-message-reference) alınabilir çeşitli iletileri hakkında ayrıntılar için.

### <a name="consuming-event-grid-events-with-azure-functions"></a>Azure işlevleri ile Event Grid olay tüketme

Bir kullanıcı Blockchain Workbench içinde gerçekleşen olaylar hakkında bildirim almak için Event Grid kullanmak isterse, Azure işlevleri'ni kullanarak Event grid'den olayların kullanabilir.

1. Oluşturma bir **Azure işlev uygulaması** Azure portalında.
2. Yeni bir işlev oluşturun.
3. Event Grid için şablonu bulun. İleti okumak için temel şablon kodunun gösterilir. Kod, gerektiği gibi değiştirin.
4. İşlev kaydedin. 
5. Event Grid, Blockchain Workbench'i'nın kaynak grubu seçin.

### <a name="consuming-event-grid-events-with-logic-apps"></a>Logic Apps ile Event Grid olay tüketme

1. Yeni bir **Azure Logic App** Azure portalında.
2. Azure Logic App portalda açılırken, bir tetikleyici seçin istenir. Seçin **Azure Event Grid kaynak olay meydana geldiğinde--**.
3. İş Akışı Tasarımcısı görüntülendiğinde, oturum açmanız istenir.
4. Aboneliği seçin. Kaynak olarak **Microsoft.EventGrid.Topics**. Seçin **kaynak adı** Azure Blockchain Workbench kaynak grubundan kaynak adını.
5. Event Grid, Blockchain Workbench'i'nın kaynak grubu seçin.

## <a name="using-service-bus-topics-for-notifications"></a>Hizmet veri yolu konuları bildirimlerini kullanma

Hizmet veri yolu konuları, Blockchain Workbench içinde gerçekleşen olaylar hakkında kullanıcılara bildirmek için kullanılabilir. 

1. Service Bus Workbench'in kaynak grubunda göz atın.
2. Seçin **konuları**.
3. Seçin **çıkış konu**.
4. Bu konu başlığına yeni bir abonelik oluşturun. Bunun için bir anahtar alın.
5. Olaylara abone olan bu abonelikten bir program oluşturun.

### <a name="consuming-service-bus-messages-with-logic-apps"></a>Logic Apps ile Service Bus iletilerini kullanma

1. Yeni bir **Azure Logic App** Azure portalında.
2. Azure Logic App portalda açılırken, bir tetikleyici seçin istenir. Tür **Service Bus** seçin ve arama kutusuna etkileşim türü için uygun bir tetikleyici ile Service Bus olmasını istediğiniz. Örneğin, **Service Bus konu aboneliğinde (Otomatik Tamamlama) bir ileti alındığında--**.
3. İş Akışı Tasarımcısı görüntülendiğinde, Service Bus bağlantı bilgilerini belirtin.
4. Aboneliğinizi seçin ve konuyu belirtin **workbench dış**.
5. Bu tetikleyiciyi iletiden kullanan uygulama mantığını geliştirin.

## <a name="notification-message-reference"></a>Bildirim iletisi başvurusu

Yapılandırmanıza bağlı olarak **messageName**, bildirim iletileri aşağıdaki ileti türlerini birine sahip.

### <a name="block-message"></a>Engelleme iletisi

Tek tek bloklar hakkında bilgi içerir. *BlockMessage* blok düzeyi bilgileri içeren bir bölüm ve işlem bilgileri ile bir bölüm içerir.

| Ad | Açıklama |
|------|-------------|
| engelle | İçeren [bilgi engelle](#block-information) |
| işlem | Bir koleksiyon içeren [işlem bilgilerini](#transaction-information) blok için |
| ConnectionID | Bağlantı için benzersiz tanımlayıcı |
| messageSchemaVersion | Mesajlaşma şema sürümü |
| messageName | **BlockMessage** |
| AdditionalInformation | Ek bilgiler sağlanmıştır |

#### <a name="block-information"></a>Blok bilgileri

| Ad              | Açıklama |
|-------------------|-------------|
| Blockıd'si           | Azure Blockchain Workbench içinde blok için benzersiz tanımlayıcı |
| BlockNumber       | Genel bir blok için benzersiz tanımlayıcı |
| BlockHash         | Blok karma |
| previousBlockHash | Önceki blok karması |
| BlockTimestamp    | Bloğun zaman damgası |

#### <a name="transaction-information"></a>İşlem bilgileri

| Ad               | Açıklama |
|--------------------|-------------|
| TransactionID      | Azure Blockchain Workbench içinde işlem için benzersiz tanımlayıcı |
| TransactionHash    | Genel muhasebe üzerinde işlem karması |
| başlangıç               | Genel muhasebe işlem kaynağı için benzersiz tanımlayıcısı |
| -                 | İşlem hedefi için bir kayıt defteri benzersiz tanımlayıcısı |
| ProvisioningStatus | Geçerli işlem için sağlama işlemini durumunu tanımlar. Olası değerler şunlardır: </br>0 – işlem veritabanı API'si tarafından oluşturuldu</br>1 – işlem defterine gönderildi</br>2 – işlem için bir kayıt defteri başarıyla kaydedildi</br>3 veya 4 - işlem için bir kayıt defteri kaydedilemedi</br>5 - işlem için bir kayıt defteri başarıyla yürütüldü |

Örnek bir *BlockMessage* blok zinciri workbench'ten:

``` json
{
    "block": {
        "blockId": 123
        "blockNumber": 1738312,
        "blockHash": "0x03a39411e25e25b47d0ec6433b73b488554a4a5f6b1a253e0ac8a200d13fffff",
        "previousBlockHash": null,
        "blockTimestamp": "2018-10-09T23:35:58Z",
    },
    "transactions": [
        {
            "transactionId": 234
            "transactionHash": "0xa4d9c95b581f299e41b8cc193dd742ef5a1d3a4ddf97bd11b80d123fec27ffff",
            "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
            "to": null,
            "provisioningStatus": 1
        },
        {
            "transactionId": 235
            "transactionHash": "0x5c1fddea83bf19d719e52a935ec8620437a0a6bdaa00ecb7c3d852cf92e1ffff",
            "from": "0xadd97e1e595916e29ea94fda894941574000ffff",
            "to": "0x9a8DDaCa9B7488683A4d62d0817E965E8f24ffff",
            "provisioningStatus": 2
        }
    ],
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "BlockMessage",
    "additionalInformation": {}
}
```

### <a name="contract-message"></a>İleti sözleşmesi

Bir sözleşme hakkında bilgi içerir. İleti sözleşmesi özelliklere sahip bir bölüm ve işlem bilgileri ile bir bölüm içerir. İşlem bölümünde bloktan sözleşmesi değiştirdiniz tüm işlemler dahildir.

| Ad | Açıklama |
|------|-------------|
| Blockıd'si | Azure Blockchain Workbench içinde blok için benzersiz tanımlayıcı |
| BlockHash | Blok karma |
| modifyingTransactions | [Değişiklik işlemleri](#modifying-transaction-information) Sözleşmesi |
| ContractId | Azure Blockchain Workbench içinde sözleşme için benzersiz tanımlayıcı |
| ContractLedgerIdentifier | Genel muhasebe sözleşme için benzersiz tanımlayıcı |
| contractProperties | [Sözleşme özellikleri](#contract-properties) |
| isNewContract | Bu sözleşmeyi yeni oluşturulmuş olup olmadığını gösterir. Olası değerler şunlardır: true: Bu sözleşme oluşturulan yeni bir sözleşme oluştu. false: Bu sözleşmenin bir sözleşme güncelleştirmesidir. |
| ConnectionID | Bağlantı için benzersiz tanımlayıcı |
| messageSchemaVersion | Mesajlaşma şema sürümü |
| messageName | **ContractMessage** |
| AdditionalInformation | Ek bilgiler sağlanmıştır |

#### <a name="modifying-transaction-information"></a>İşlem bilgilerini değiştirme

| Ad               | Açıklama |
|--------------------|-------------|
| TransactionID | Azure Blockchain Workbench içinde işlem için benzersiz tanımlayıcı |
| TransactionHash | Genel muhasebe üzerinde işlem karması |
| başlangıç | Genel muhasebe işlem kaynağı için benzersiz tanımlayıcısı |
| - | İşlem hedefi için bir kayıt defteri benzersiz tanımlayıcısı |

#### <a name="contract-properties"></a>Sözleşme özellikleri

| Ad               | Açıklama |
|--------------------|-------------|
| WorkflowPropertyId | Azure Blockchain Workbench içinde iş akışı özellik için benzersiz tanımlayıcı |
| ad | İş akışı özelliğin adı |
| value | İş akışı özelliğinin değeri |

Örnek bir *ContractMessage* blok zinciri workbench'ten:

``` json
{
    "blockId": 123,
    "blockhash": "0x03a39411e25e25b47d0ec6433b73b488554a4a5f6b1a253e0ac8a200d13fffff",
    "modifyingTransactions": [
        {
            "transactionId": 234,
            "transactionHash": "0x5c1fddea83bf19d719e52a935ec8620437a0a6bdaa00ecb7c3d852cf92e1ffff",
            "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
            "to": "0xf8559473b3c7197d59212b401f5a9f07ffff"
        },
        {
            "transactionId": 235,
            "transactionHash": "0xa4d9c95b581f299e41b8cc193dd742ef5a1d3a4ddf97bd11b80d123fec27ffff",
            "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
            "to": "0xf8559473b3c7197d59212b401f5a9f07b429ffff"
        }
    ],
    "contractId": 111,
    "contractLedgerIdentifier": "0xf8559473b3c7197d59212b401f5a9f07b429ffff",
    "contractProperties": [
        {
            "workflowPropertyId": 1,
            "name": "State",
            "value": "0"
        },
        {
            "workflowPropertyId": 2,
            "name": "Description",
            "value": "1969 Dodge Charger"
        },
        {
            "workflowPropertyId": 3,
            "name": "AskingPrice",
            "value": "30000"
        },
        {
            "workflowPropertyId": 4,
            "name": "OfferPrice",
            "value": "0"
        },
        {
            "workflowPropertyId": 5,
            "name": "InstanceAppraiser",
            "value": "0x0000000000000000000000000000000000000000"
        },
        {
            "workflowPropertyId": 6,
            "name": "InstanceBuyer",
            "value": "0x0000000000000000000000000000000000000000"
        },
        {
            "workflowPropertyId": 7,
            "name": "InstanceInspector",
            "value": "0x0000000000000000000000000000000000000000"
        },
        {
            "workflowPropertyId": 8,
            "name": "InstanceOwner",
            "value": "0x9a8DDaCa9B7488683A4d62d0817E965E8f24ffff"
        },
        {
            "workflowPropertyId": 9,
            "name": "ClosingDayOptions",
            "value": "[21,48,69]"
        }
    ],
    "isNewContract": false,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "ContractMessage",
    "additionalInformation": {}
}
```

### <a name="event-message-contract-function-invocation"></a>Olay iletisi: Sözleşme işlevi çağırma

Bir sözleşme işlevi çağrıldığında işlev adı, parametreleri giriş ve çağıran işlevin gibi bilgileri içerir.

| Ad | Açıklama |
|------|-------------|
| EventName                   | **ContractFunctionInvocation** |
| çağıran                      | [Arayan bilgileri](#caller-information) |
| ContractId                  | Azure Blockchain Workbench içinde sözleşme için benzersiz tanımlayıcı |
| ContractLedgerIdentifier    | Genel muhasebe sözleşme için benzersiz tanımlayıcı |
| functionName                | İşlevin adı |
| parametreler                  | [Parametre bilgileri](#parameter-information) |
| İşlem                 | İşlem bilgileri |
| inTransactionSequenceNumber | İşlem bloğundaki sıra numarası |
| ConnectionID                | Bağlantı için benzersiz tanımlayıcı |
| messageSchemaVersion        | Mesajlaşma şema sürümü |
| messageName                 | **EventMessage** |
| AdditionalInformation       | Ek bilgiler sağlanmıştır |

#### <a name="caller-information"></a>Arayan bilgileri

| Ad | Açıklama |
|------|-------------|
| type | Arayanın gibi bir kullanıcı veya sözleşme türü |
| id | Arayanın içinde Azure Blockchain Workbench için benzersiz tanımlayıcı |
| ledgerIdentifier | Çağrı üzerinde genel benzersiz tanıtıcısı |

#### <a name="parameter-information"></a>Parametre bilgileri

| Ad | Açıklama |
|------|-------------|
| ad | Parametre adı |
| value | Parametre değeri |

#### <a name="event-message-transaction-information"></a>Olay iletisi işlem bilgileri

| Ad               | Açıklama |
|--------------------|-------------|
| TransactionID      | Azure Blockchain Workbench içinde işlem için benzersiz tanımlayıcı |
| TransactionHash    | Genel muhasebe üzerinde işlem karması |
| başlangıç               | Genel muhasebe işlem kaynağı için benzersiz tanımlayıcısı |
| -                 | İşlem hedefi için bir kayıt defteri benzersiz tanımlayıcısı |

Örnek bir *EventMessage ContractFunctionInvocation* blok zinciri workbench'ten:

``` json
{
    "eventName": "ContractFunctionInvocation",
    "caller": {
        "type": "User",
        "id": 21,
        "ledgerIdentifier": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60ffff"
    },
    "contractId": 34,
    "contractLedgerIdentifier": "0xf8559473b3c7197d59212b401f5a9f07b429ffff",
    "functionName": "Modify",
    "parameters": [
        {
            "name": "description",
            "value": "a new description"
        },
        {
            "name": "price",
            "value": "4567"
        }
    ],
    "transaction": {
        "transactionId": 234,
        "transactionHash": "0x5c1fddea83bf19d719e52a935ec8620437a0a6bdaa00ecb7c3d852cf92e1ffff",
        "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
        "to": "0xf8559473b3c7197d59212b401f5a9f07b429ffff"
    },
    "inTransactionSequenceNumber": 1,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "EventMessage",
    "additionalInformation": { }
}
```

### <a name="event-message-application-ingestion"></a>Olay iletisi: Uygulama alma

Workbench için bir uygulama yüklendiğinde adı ve sürümü gibi bilgileri içerir.

| Ad | Açıklama |
|------|-------------|
| EventName | **ApplicationIngestion** |
| ApplicationId | Azure Blockchain Workbench içindeki uygulama için benzersiz tanımlayıcı |
| ApplicationName | Uygulama adı |
| ApplicationDisplayName | Uygulamanın görünen adı |
| ApplicationVersion | Uygulama sürümü |
| applicationDefinitionLocation | Uygulama yapılandırma dosyasının bulunduğu URL'si |
| contractCodes | Koleksiyonu [sözleşme kodları](#contract-code-information) uygulama |
| ApplicationRoles | Koleksiyonu [uygulama rolleri](#application-role-information) uygulama |
| applicationWorkflows | Koleksiyonu [uygulama iş akışlarını](#application-workflow-information) uygulama |
| ConnectionID | Bağlantı için benzersiz tanımlayıcı |
| messageSchemaVersion | Mesajlaşma şema sürümü |
| messageName | **EventMessage** |
| AdditionalInformation | Burada sağlanan ek bilgiler uygulama iş akışı durumlarını ve geçiş bilgilerini içerir. |

#### <a name="contract-code-information"></a>Kod bilgi Sözleşmesi

| Ad | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içinde sözleşme kod dosyası için benzersiz tanımlayıcı |
| LedgerId | Azure Blockchain Workbench içinde bir kayıt defteri için benzersiz tanımlayıcı |
| location | Sözleşme kod dosyasının bulunduğu URL'si |

#### <a name="application-role-information"></a>Uygulama rol bilgileri

| Ad | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içinde uygulama rolü için benzersiz tanımlayıcı |
| ad | Uygulama rolü adı |

#### <a name="application-workflow-information"></a>Uygulama iş akışı bilgileri

| Ad | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içinde uygulama iş akışı için benzersiz tanımlayıcı |
| ad | Uygulama iş akışı adı |
| displayName | Uygulama iş akışı görünen adı |
| işlevler | Koleksiyonu [uygulama iş akışı için işlevleri](#workflow-function-information)|
| durumları | Koleksiyonu [uygulama iş akışı durumları](#workflow-state-information) |
| properties | Uygulama [iş akışı özellikleri bilgileri](#workflow-property-information) |

##### <a name="workflow-function-information"></a>İş akışı işlevi bilgileri

| Ad | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içinde uygulama iş akışı işlevi için benzersiz tanımlayıcı |
| ad | İşlev adı |
| parametreler | İşlevi için parametreler |

##### <a name="workflow-state-information"></a>İş akışı durumu bilgileri

| Ad | Açıklama |
|------|-------------|
| ad | Eyalet adı |
| displayName | Durum görünen adı |
| Stil | Durum stili (başarı veya hata) |

##### <a name="workflow-property-information"></a>İş akışı özellik bilgileri

| Ad | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içinde uygulama iş akışı özelliği için benzersiz tanımlayıcı |
| ad | Özellik adı |
| type | Özellik türü |

Örnek bir *EventMessage ApplicationIngestion* blok zinciri workbench'ten:

``` json
{
    "eventName": "ApplicationIngestion",
    "applicationId": 31,
    "applicationName": "AssetTransfer",
    "applicationDisplayName": "Asset Transfer",
    "applicationVersion": “1.0”,
    "applicationDefinitionLocation": "http://url"
    "contractCodes": [
        {
            "id": 23,
            "ledgerId": 1,
            "location": "http://url"
        }
    ],
    "applicationRoles": [
            {
                "id": 134,
                "name": "Buyer"
            },
            {
                "id": 135,
                "name": "Seller"
            }
       ],
    "applicationWorkflows": [
        {
            "id": 89,
            "name": "AssetTransfer",
            "displayName": "Asset Transfer",
            "functions": [
                {
                    "id": 912,
                    "name": "",
                    "parameters": [
                        {
                            "name": "description",
                            "type": {
                                "name": "string"
                             }
                        },
                        {
                            "name": "price",
                            "type": {
                                "name": "int"
                            }
                        }
                    ]
                },
                {
                    "id": 913,
                    "name": "modify",
                    "parameters": [
                        {
                            "name": "description",
                            "type": {
                                "name": "string"
                             }
                        },
                        {
                            "name": "price",
                            "type": {
                                "name": "int"
                            }
                        }
                    ]
                }
            ],
            "states": [ 
                 {
                      "name": "Created",
                      "displayName": "Created",
                      "style" : "Success"
                 },
                 {
                      "name": "Terminated",
                      "displayName": "Terminated",
                      "style" : "Failure"
                 }
            ],
            "properties": [
                {
                    "id": 879,
                    "name": "Description",
                    "type": {
                                "name": "string"
                     }
                },
                {
                    "id": 880,
                    "name": "Price",
                    "type": {
                                "name": "int"
                     }
                }
            ]
        }
    ]
    "connectionId": [ ],
    "messageSchemaVersion": "1.0.0",
    "messageName": "EventMessage",
    "additionalInformation":
        {
            "states" :
            [
                {
                    "Name": "BuyerAccepted",
                    "Transitions": [
                        {
                            "DisplayName": "Accept"
                            "AllowedRoles": [ ],
                            "AllowedInstanceRoles": [ "InstanceOwner" ],
                            "Function": "Accept",
                            "NextStates": [ "SellerAccepted" ]
                        }
                    ]
                }
            ]
        }
}
```

### <a name="event-message-role-assignment"></a>Olay iletisi: Rol ataması

Bir kullanıcı rol ataması ve karşılık gelen uygulama ve rol adını gerçekleştiren gibi Workbench, bir rol atandığında bilgileri içerir.

| Ad | Açıklama |
|------|-------------|
| EventName | **RoleAssignment** |
| ApplicationId | Azure Blockchain Workbench içindeki uygulama için benzersiz tanımlayıcı |
| ApplicationName | Uygulama adı |
| ApplicationDisplayName | Uygulamanın görünen adı |
| ApplicationVersion | Uygulama sürümü |
| applicationRole        | Hakkında bilgi [uygulama rolü](#roleassignment-application-role) |
| atayan               | Hakkında bilgi [atayan](#roleassignment-assigner) |
| atanan               | Hakkında bilgi [atanan](#roleassignment-assignee) |
| ConnectionID           | Bağlantı için benzersiz tanımlayıcı |
| messageSchemaVersion   | Mesajlaşma şema sürümü |
| messageName            | **EventMessage** |
| AdditionalInformation  | Ek bilgiler sağlanmıştır |

#### <a name="roleassignment-application-role"></a>RoleAssignment uygulama rolü

| Ad | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içinde uygulama rolü için benzersiz tanımlayıcı |
| ad | Uygulama rolü adı |

#### <a name="roleassignment-assigner"></a>RoleAssignment atayan

| Ad | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içinde kullanıcının benzersiz tanımlayıcısı |
| type | Atayan türü |
| ChainIdentifier | Genel muhasebe kullanıcının benzersiz tanımlayıcısı |

#### <a name="roleassignment-assignee"></a>RoleAssignment atanan

| Ad | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içinde kullanıcının benzersiz tanımlayıcısı |
| type | Atanan türü |
| ChainIdentifier | Genel muhasebe kullanıcının benzersiz tanımlayıcısı |

Örnek bir *EventMessage RoleAssignment* blok zinciri workbench'ten:

``` json
{
    "eventName": "RoleAssignment",
    "applicationId": 31,
    "applicationName": "AssetTransfer",
    "applicationDisplayName": "Asset Transfer",
    "applicationVersion": “1.0”,
    "applicationRole": {
        "id": 134,
        "name": "Buyer"
    },
    "assigner": {
        "id": 1,
        "type": null,
        "chainIdentifier": "0xeFFC7766d38aC862d79706c3C5CEEf089564ffff"
    },
    "assignee": {
        "id": 3,
        "type": null,
        "chainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f24ffff"
    },
    "connectionId": [ ],
    "messageSchemaVersion": "1.0.0",
    "messageName": "EventMessage",
    "additionalInformation": { }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Akıllı sözleşme tümleştirme desenleri](integration-patterns.md)
