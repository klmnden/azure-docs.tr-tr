---
title: Azure Blockchain çalışma ekranı iletileri genel bakış
description: İletileri Azure Blockchain çalışma ekranı içinde kullanma konusuna genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/16/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 8d9b9c74ead54280d82bf2b0f9a2232723b2af63
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="azure-blockchain-workbench-messages-overview"></a>Azure Blockchain çalışma ekranı iletileri genel bakış

Bir REST API sağlamanın yanı sıra, Azure Blockchain çalışma ekranı ileti tabanlı tümleştirme de sağlar. Çalışma ekranı aşağı akış tüketicilerin veri alma veya bu olaylara göre eyleme etkinleştirme Azure olay kılavuz üzerinden defter merkezli olayları yayımlar. Güvenilir Mesajlaşma gerektiren istemcilerinin Azure Blockchain çalışma ekranı iletileri bir Azure Service Bus uç nokta için de sunar.

Geliştiriciler ayrıca kullanıcıları oluşturun, sözleşmeleri oluşturmak ve bir muhasebe sözleşmelerinde güncelleştirmek için başlatma işlemleri iletişim dış sistemler sahip olma yeteneği ilgi belirtmiş. Bu işlevsellik şu anda genel önizlemede gösterilmez, ancak bu olanağı sunan bir örnek bulunabilir [ http://aka.ms/blockchain-workbench-integration-sample ](http://aka.ms/blockchain-workbench-integration-sample).


## <a name="event-notifications"></a>Olay bildirimleri

Olay bildirimleri, kullanıcılara ve çalışma ekranı ve bağlı blockchain ağ gerçekleşen olayları aşağı akış sistemlerini bildirmek için kullanılabilir. Olay bildirimleri doğrudan kod içinde kullanılan veya Logic Apps ve akış gibi araçlarla aşağı akış sistemlere veri akışını tetiklemek için kullanılır.

Bkz: [bildirim iletisi başvurusu](#notification-message-reference) alınabilir çeşitli iletiler ayrıntıları için.

### <a name="consuming-event-grid-events-with-azure-functions"></a>Azure işlevleri ile olay kılavuz olaylarını kullanma

Bir kullanıcı Blockchain çalışma ekranı içinde gerçekleşen olayları hakkında bildirim almak için olay kılavuz kullanmak isterse, Azure işlevlerini kullanarak olay kılavuz olaylarından kullanmasını sağlayabilirsiniz.

1. Oluşturma bir **Azure işlev uygulaması** Azure portalında.
2. Yeni bir işlev oluşturun.
3. Olay grubu için şablonu bulun. İleti okumak için temel şablonu kodu gösterilir. Kodu gerektiği gibi değiştirin.
4. İşlev kaydedin. 
5. Olay kılavuz Blockchain çalışma'nın kaynak grubundan seçin.

### <a name="consuming-event-grid-events-with-logic-apps"></a>Logic Apps ile olay kılavuz olaylarını kullanma

1.  Yeni bir **Azure mantıksal uygulama** Azure portalında.
2.  Azure mantıksal uygulama portalda açarken bir tetikleyici seçmeniz istenir. Seçin **Azure olay kaynağı olay gerçekleştiğinde kılavuz--**.
3. İş Akışı Tasarımcısı'nı görüntülendiğinde, oturum açmak için istenir.
4. Aboneliği seçin. Kaynak olarak **Microsoft.EventGrid.Topics**. Seçin **kaynak adı** Azure Blockchain çalışma ekranı kaynak grubundan kaynağının adı.
5. Olay kılavuz Blockchain çalışma'nın kaynak grubundan seçin.

## <a name="using-service-bus-topics-for-notifications"></a>Bildirimler için Service Bus konu başlıklarını kullanma

Hizmet veri yolu konuları Blockchain çalışma ekranı içinde gerçekleşecek olaylar hakkında kullanıcılara bildirmek için kullanılır. 

1.  Hizmet veri yolu çalışma ekranı'nın kaynak grubunda göz atın.
2.  Seçin **konuları**.
3.  Seçin **çalışma ekranı dış**.
4.  Bu konu için yeni bir abonelik oluşturun. Bir anahtar için alın.
5.  Bu abonelikten olayları kaydeder bir program oluşturun.

### <a name="consuming-service-bus-messages-with-logic-apps"></a>Hizmet veri yolu ileti Logic Apps ile kullanma

1. Yeni bir **Azure mantıksal uygulama** Azure portalında.
2.  Azure mantıksal uygulama portalda açarken bir tetikleyici seçmeniz istenir. Tür **Service Bus** seçin ve arama kutusu etkileşim türü için uygun tetikleyici Service Bus ile olmasını istediğiniz. Örneğin, **Service Bus bir konu aboneliği (Otomatik Tamamlama) bir ileti alındığında--**.
3. İş Akışı Tasarımcısı'nı görüntülendiğinde, Service Bus bağlantı bilgilerini belirtin.
4. Aboneliğinizi seçin ve konu belirtin **çalışma ekranı dış**.
5. Bu tetikleyici iletiden kullanır, uygulamanız için mantığı geliştirin.

## <a name="notification-message-reference"></a>Bildirim iletisi başvurusu

Bildirim iletileri OperationName bağlı olarak, aşağıdaki ileti türlerini birine sahip.

### <a name="accountcreated"></a>AccountCreated

Yeni bir hesap için belirtilen zinciri eklenecek istenen gösterir.

| Ad    | Açıklama  |
|----------|--------------|
| UserId  | Oluşturulan kullanıcı kimliği |
| ChainIdentifier | Blockchain ağda oluşturulan kullanıcı adresi. Ethereum içinde bu kullanıcının "üzerinde zinciri" adresi olacaktır. |

``` csharp
public class NewAccountRequest : MessageModelBase
{
  public int UserID { get; set; }
  public string ChainIdentifier { get; set; }
}
```

### <a name="contractinsertedorupdated"></a>ContractInsertedOrUpdated

Bir istek eklemek veya dağıtılmış bir muhasebe sözleşmesinde güncelleştirmek için yapılan gösterir.

| Ad | Açıklama |
|-----|--------------|
| ChainID | İstekle ilişkili zinciri için benzersiz bir tanımlayıcı.|
  Blockıd'si | Bir blok defter benzersiz tanımlayıcısı.|
  ContractId | Sözleşme için benzersiz bir tanımlayıcı.|
  ContractAddress |       Muhasebe sözleşme adresi.|
  TransactionHash  |     Muhasebe işlem karması.|
  OriginatingAddress |   İşlem gönderen adresi.|
  EylemAdı       |     Eylemin adı.|
  IsUpdate        |      Bu bir güncelleştirme olup olmadığını tanımlar.|
  Parametreler       |     Bir eylem gönderilen parametreler ada, değere ve veri türünü tanımlamak nesneleri listesi.|
  TopLevelInputParams |  Bir sözleşme bir veya daha fazla diğer sözleşmeleri bağlandığı senaryolarda, bunlar en üst düzey sözleşmeden parametreleridir. |

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

Belirli bir sözleşme dağıtılmış bir muhasebe üzerinde bir eylem yürütme için bir istek yapıldığını gösterir.

| Ad                     | Açıklama                                                                                                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ContractActionId         | Bu sözleşme eylemi için benzersiz tanımlayıcı                                                                                                                                |
| ChainIdentifier          | Zincir için benzersiz tanımlayıcı                                                                                                                                           |
| ConnectionID             | Bağlantı için benzersiz tanımlayıcı                                                                                                                                      |
| UserChainIdentifier      | Blockchain ağda oluşturulan kullanıcı adresi. Ethereum içinde bu kullanıcının "üzerinde zinciri" adresi olacaktır.                                                     |
| ContractLedgerIdentifier | Muhasebe sözleşme adresi.                                                                                                                                        |
| WorkflowFunctionName     | İş akışı işlevinin adı.                                                                                                                                                |
| WorkflowName             | İş akışının adı.                                                                                                                                                         |
| WorkflowBlobStorageURL   | Blob depolama sözleşmede URL'si.                                                                                                                                      |
| ContractActionParameters | Sözleşme eylem parametreleri.                                                                                                                                           |
| TransactionHash          | Muhasebe işlem karması.                                                                                                                                    |
| Sağlama Durumu      | Eylem geçerli sağlama durumu.</br>0 – oluşturuldu</br>1 – işlemi</br>2 – tamamlayın</br> Tam bir onay defterinden gösterir, bu gibi başarıyla eklendi.                                               |
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

Bir isteği belirli bir dağıtılmış muhasebe kullanıcı bakiyesi güncelleştirmek için yapılan gösterir.

> [!NOTE]
> Bu ileti hesaplarının fon gerektiren bu defterlerinin oluşturulur.
> 

| Ad    | Açıklama                              |
|---------|------------------------------------------|
| Adres | Finanse kullanıcı adresi. |
| Denge | Kullanıcı Bakiye Bakiye.         |
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

Bir isteğin bir blok üzerinde dağıtılmış bir muhasebe eklemek için yapılan ileti gösterir.

| Ad           | Açıklama                                                            |
|----------------|------------------------------------------------------------------------|
| ChainId        | Blok eklendiği zinciri benzersiz tanımlayıcısı.             |
| Blockıd'si        | Azure Blockchain çalışma ekranı bloğunda benzersiz tanımlayıcısı. |
| BlockHash      | Blok karma.                                                 |
| BlockTimeStamp | Blok zaman damgası.                                            |

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

İleti bir işlem üzerinde dağıtılmış bir muhasebe eklemek için bir istek ayrıntıları sağlar.

| Ad            | Açıklama                                                            |
|-----------------|------------------------------------------------------------------------|
| ChainId         | Blok eklendiği zinciri benzersiz tanımlayıcısı.             |
| Blockıd'si         | Azure Blockchain çalışma ekranı bloğunda benzersiz tanımlayıcısı. |
| TransactionHash | İşlem karması.                                           |
| Kimden            | İşlem gönderen adresi.                      |
| Alıcı              | Hareketin hedeflenen alıcı adresi.              |
| Değer           | Harekete dahil değeri.                                 |
| IsAppBuilderTx  | Bu bir Blockchain çalışma ekranı işlem olup olmadığını tanımlar.                         |

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

Ayrıntılar için bir sözleşmede zinciri tanımlayıcı atamada sağlar. Örneğin, Ethereum blockchain içinde muhasebe sözleşmesinde adresidir.

| Ad            | Açıklama                                                                       |
|-----------------|-----------------------------------------------------------------------------------|
| ContractId      | Azure Blockchain çalışma ekranı içinde sözleşme için benzersiz tanımlayıcı budur. |
| ChainIdentifier | Zincirdeki sözleşme tanımlayıcısıdır.                             |

``` csharp
public class AssignContractChainIdentifierRequest : MessageModelBase
{
    public int ContractId { get; set; }
    public string ChainIdentifier { get; set; }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain çalışma ekranı mimarisi](blockchain-workbench-architecture.md)