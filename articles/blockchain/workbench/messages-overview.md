---
title: Azure Blockchain Workbench iletileri tümleştirmesine genel bakış
description: Azure Blockchain Workbench uygulamasında mesajları genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: dcf31bd6b128115962e30dd6653364c4cd65047d
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48243289"
---
# <a name="azure-blockchain-workbench-messaging-integration"></a>Azure Blockchain Workbench ile tümleştirme Mesajlaşma

REST API yanı sıra, Azure Blockchain Workbench de ileti tabanlı bir tümleştirme sağlar. Workbench, muhasebe merkezli olayları Azure Event Grid, veri alma veya bu olaylara göre eylemde bulunmak aşağı akış tüketiciler etkinleştirme aracılığıyla yayımlar. Güvenilir Mesajlaşma gerekli istemcilerinin Azure Blockchain Workbench iletileri bir Azure Service Bus uç noktası için de sunar.

Geliştiriciler Ayrıca dış sistemler kullanıcıları oluşturun, sözleşmeler oluşturun ve bir kayıt defteri sözleşmelerinde güncelleştirmek için başlatma işlemleri iletişim yeteneğini gösterdiğiniz ilgi belirtmiş. Bu işlevsellik şu anda genel önizlemede gösterilmez, ancak söz konusu özellik sunan bir örnek bulabilirsiniz [ http://aka.ms/blockchain-workbench-integration-sample ](http://aka.ms/blockchain-workbench-integration-sample).

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

1.  Yeni bir **Azure Logic App** Azure portalında.
2.  Azure Logic App portalda açılırken, bir tetikleyici seçin istenir. Seçin **Azure Event Grid kaynak olay meydana geldiğinde--**.
3. İş Akışı Tasarımcısı görüntülendiğinde, oturum açmanız istenir.
4. Aboneliği seçin. Kaynak olarak **Microsoft.EventGrid.Topics**. Seçin **kaynak adı** Azure Blockchain Workbench kaynak grubundan kaynak adını.
5. Event Grid, Blockchain Workbench'i'nın kaynak grubu seçin.

## <a name="using-service-bus-topics-for-notifications"></a>Hizmet veri yolu konuları bildirimlerini kullanma

Hizmet veri yolu konuları, Blockchain Workbench içinde gerçekleşen olaylar hakkında kullanıcılara bildirmek için kullanılabilir. 

1.  Service Bus Workbench'in kaynak grubunda göz atın.
2.  Seçin **konuları**.
3.  Seçin **workbench dış**.
4.  Bu konu başlığına yeni bir abonelik oluşturun. Bunun için bir anahtar alın.
5.  Olaylara abone olan bu abonelikten bir program oluşturun.

### <a name="consuming-service-bus-messages-with-logic-apps"></a>Logic Apps ile Service Bus iletilerini kullanma

1. Yeni bir **Azure Logic App** Azure portalında.
2. Azure Logic App portalda açılırken, bir tetikleyici seçin istenir. Tür **Service Bus** seçin ve arama kutusuna etkileşim türü için uygun bir tetikleyici ile Service Bus olmasını istediğiniz. Örneğin, **Service Bus konu aboneliğinde (Otomatik Tamamlama) bir ileti alındığında--**.
3. İş Akışı Tasarımcısı görüntülendiğinde, Service Bus bağlantı bilgilerini belirtin.
4. Aboneliğinizi seçin ve konuyu belirtin **workbench dış**.
5. Bu tetikleyiciyi iletiden kullanan uygulama mantığını geliştirin.

## <a name="notification-message-reference"></a>Bildirim iletisi başvurusu

Yapılandırmanıza bağlı olarak **OperationName**, bildirim iletileri aşağıdaki ileti türlerini birine sahip.

### <a name="accountcreated"></a>AccountCreated

Yeni bir hesap için belirtilen zinciri eklenecek istenen gösterir.

| Ad    | Açıklama  |
|----------|--------------|
| UserId  | Oluşturulan kullanıcı kimliği. |
| ChainIdentifier | Blok zinciri ağ üzerinde oluşturulan kullanıcı adresi. Ethereum bu kullanıcının olacaktır **zincir** adresi. |

``` csharp
public class NewAccountRequest : MessageModelBase
{
  public int UserID { get; set; }
  public string ChainIdentifier { get; set; }
}
```

### <a name="contractinsertedorupdated"></a>ContractInsertedOrUpdated

Bir istek eklemek veya dağıtılmış bir defter sözleşmesinde güncelleştirmek için yapılan gösterir.

| Ad | Açıklama |
|-----|--------------|
| ChainID | İstekle ilişkili zinciri için benzersiz bir tanımlayıcı.|
| Blockıd'si | Genel bir blok için benzersiz tanımlayıcı.|
| ContractId | Anlaşma için benzersiz bir tanımlayıcı.|
| ContractAddress |       Genel muhasebe sözleşme adresi.|
| TransactionHash  |     Genel muhasebe üzerinde işlem karması.|
| OriginatingAddress |   İşlem kaynağının adresi.|
| actionName       |     Eylemin adı.|
| IsUpdate        |      Bu güncelleştirme olup olmadığını tanımlar.|
| Parametreler       |     Bir liste parametreleri için bir eylem gönderilen ada, değere ve veri türünü tanımlayan nesne.|
| TopLevelInputParams |  Bir sözleşme bir veya daha fazla diğer sözleşmeleri nerede bağlı senaryolarda, üst düzey sözleşmesi Parametreler şunlardır. |

``` csharp
public class ContractInsertOrUpdateRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public int ContractId { get; set; }
    public string ContractAddress { get; set; }
    public string TransactionHash { get; set; }
    public string OriginatingAddress { get; set; }
    public string ActionName { get; set; }
    public bool IsUpdate { get; set; }
    public List<ContractProperty> Parameters { get; set; }
    public bool IsTopLevelUpdate { get; set; }
    public List<ContractInputParameter> TopLevelInputParams { get; set; }
}
```

#### <a name="updatecontractaction"></a>UpdateContractAction

Belirli bir dağıtılmış bir defter sözleşmesinde bir eylem yürütme için bir istek yapıldığını gösterir.

| Ad                     | Açıklama                                                                                                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ContractActionId         | Bu sözleşme eylem için benzersiz tanımlayıcı                                                                                                                                |
| ChainIdentifier          | Zinciri için benzersiz tanımlayıcı                                                                                                                                           |
| ConnectionID             | Bağlantı için benzersiz tanımlayıcı                                                                                                                                      |
| UserChainIdentifier      | Blok zinciri ağ üzerinde oluşturulan kullanıcı adresi. Ethereum bu kullanıcının "üzerinde zinciri" adresini olacaktır.                                                     |
| ContractLedgerIdentifier | Genel muhasebe sözleşme adresi.                                                                                                                                        |
| WorkflowFunctionName     | İş akışı işlevinin adı.                                                                                                                                                |
| WorkflowName             | İş akışının adı.                                                                                                                                                         |
| WorkflowBlobStorageURL   | Blob depolama alanındaki sözleşme URL'si.                                                                                                                                      |
| ContractActionParameters | Sözleşme eylemi için parametreleri.                                                                                                                                           |
| TransactionHash          | Genel muhasebe üzerinde işlem karması.                                                                                                                                    |
| Sağlama Durumu      | Eylem geçerli sağlama durumu.</br>0 – oluşturuldu</br>1 – işleminde</br>2 – tamamlayın</br> Eksiksiz bir onay defterinden gösterir, bu gibi başarıyla eklendi.                                               |
|                          |                                                                                                                                                                               |

```csharp
public class ContractActionRequest : MessageModelBase
{
    public int ContractActionId { get; set; }
    public int ConnectionId { get; set; }
    public string UserChainIdentifier { get; set; }
    public string ContractLedgerIdentifier { get; set; }
    public string WorkflowFunctionName { get; set; }
    public string WorkflowName { get; set; }
    public string WorkflowBlobStorageURL { get; set; }
    public IEnumerable<ContractActionParameter> ContractActionParameters { get; set; }
    public string TransactionHash { get; set; }
    public int ProvisioningStatus { get; set; }
}
```

### <a name="updateuserbalance"></a>UpdateUserBalance

Bir istek belirli bir dağıtılmış kayıt defteri kullanıcı bakiyeye güncelleştirmek için yapılan gösterir.

> [!NOTE]
> Bu ileti, hesaplarını fon gerektiren bu defterlerinin oluşturulur.
> 

| Ad    | Açıklama                              |
|---------|------------------------------------------|
| Adres | Fon kullanıcı adresi. |
| Bakiye | Kullanıcı Bakiye bakiyesi         |
| ChainID | Zincir benzersiz tanımlayıcısı.     |


``` csharp
public class UpdateUserBalanceRequest : MessageModelBase
{
    public string Address { get; set; }
    public decimal Balance { get; set; }
    public int ChainID { get; set; }
}
```

### <a name="insertblock"></a>InsertBlock

Bir istek üzerinde dağıtılmış bir defter bloğu eklemek üzere yapılan ileti gösterir.

| Ad           | Açıklama                                                            |
|----------------|------------------------------------------------------------------------|
| ChainId        | Blok eklendiği zinciri benzersiz tanımlayıcısı.             |
| Blockıd'si        | Azure Blockchain Workbench içinde blok için benzersiz tanımlayıcı. |
| BlockHash      | Blok karma.                                                 |
| BlockTimeStamp | Bloğun zaman damgası.                                            |

``` csharp
public class InsertBlockRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public string BlockHash { get; set; }
    public int BlockTimestamp { get; set; }
}
```

### <a name="inserttransaction"></a>InsertTransaction

İleti, işlem üzerinde dağıtılmış bir defter ekleme isteği hakkında ayrıntılar sağlar.

| Ad            | Açıklama                                                            |
|-----------------|------------------------------------------------------------------------|
| ChainId         | Blok eklendiği zinciri benzersiz tanımlayıcısı.             |
| Blockıd'si         | Azure Blockchain Workbench içinde blok için benzersiz tanımlayıcı. |
| TransactionHash | İşlem karması.                                           |
| Kimden            | İşlem kaynağının adresi.                      |
| Alıcı              | İşlem hedeflenen alıcı adresi.              |
| Değer           | Harekete dahil değeri.                                 |
| IsAppBuilderTx  | Bu Blockchain Workbench'i işlem olup olmadığını tanımlar.                         |

``` csharp
public class InsertTransactionRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public string TransactionHash { get; set; }
    public string From { get; set; }
    public string To { get; set; }
    public decimal Value { get; set; }
    public bool IsAppBuilderTx { get; set; }
}
```

### <a name="assigncontractchainidentifier"></a>AssignContractChainIdentifier

Ayrıntılar zinciri tanımlayıcısı atamada sözleşme sağlar. Örneğin, Ethereum blok zincirine yatırım, muhasebe sözleşmesinde adresidir.

| Ad            | Açıklama                                                                       |
|-----------------|-----------------------------------------------------------------------------------|
| ContractId      | Azure Blockchain Workbench içinde sözleşme için benzersiz tanımlayıcı budur. |
| ChainIdentifier | Bu zincir sözleşme tanımlayıcısıdır.                             |

``` csharp
public class AssignContractChainIdentifierRequest : MessageModelBase
{
    public int ContractId { get; set; }
    public string ChainIdentifier { get; set; }
}
```

## <a name="classes-used-by-message-types"></a>İleti türleri tarafından kullanılan sınıflar

### <a name="messagemodelbase"></a>MessageModelBase

Tüm iletileri için temel modeli.

| Ad          | Açıklama                          |
|---------------|--------------------------------------|
| OperationName | İşlemin adı.           |
| RequestId     | İstek için benzersiz bir tanımlayıcı. |

``` csharp
public class MessageModelBase
{
    public string OperationName { get; set; }
    public string RequestId { get; set; }
}
```

### <a name="contractinputparameter"></a>ContractInputParameter

Ad, değer ve bir parametresinin türü içerir.

| Ad  | Açıklama                 |
|-------|-----------------------------|
| Ad  | Parametrenin adı.  |
| Değer | Parametre değeri. |
| Tür  | Parametrenin türü.  |

``` csharp
public class ContractInputParameter
{
    public string Name { get; set; }
    public string Value { get; set; }
    public string Type { get; set; }
}
```

#### <a name="contractproperty"></a>ContractProperty

Kimlik, ad, değer ve bir özelliğin türünü içerir.

| Ad  | Açıklama                |
|-------|----------------------------|
| Kimlik    | Özellik kimliği.    |
| Ad  | Özelliğin adı.  |
| Değer | Özelliğin değeri. |
| Tür  | Özelliğin türü.  |

``` csharp
public class ContractProperty
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Value { get; set; }
    public string DataType { get; set; }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Akıllı sözleşme tümleştirme desenleri](integration-patterns.md)