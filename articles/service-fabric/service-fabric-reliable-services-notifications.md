---
title: Reliable Services bildirimlerini | Microsoft Docs
description: Service Fabric güvenilir hizmetler bildirimleri için kavramsal belgelerde
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: chackdan
editor: masnider,vturecek
ms.assetid: cdc918dd-5e81-49c8-a03d-7ddcd12a9a76
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/29/2017
ms.author: mcoskun
ms.openlocfilehash: d009749b7bc31595be26124b9d1eee7666e95bd4
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551682"
---
# <a name="reliable-services-notifications"></a>Reliable Services bildirimleri
Bildirimler, ilginizi çeken bir nesneye yapılan değişiklikleri izlemek istemcileri olanak tanır. Bildirimleri iki tür nesne destekler: *Güvenilir durum Yöneticisi* ve *güvenilir bir sözlük*.

Bildirimlerin kullanılmasıyla ilgili yaygın nedenleri şunlardır:

* Yapı, gerçekleştirilmiş görünümler, ikincil dizinler gibi veya filtrelenmiş yinelemenin durumu görünümlerini toplanır. Örnek, güvenilir bir sözlükteki tüm anahtarların sıralanmış bir dizindir.
* Gönderen izleme verilerini, son bir saat içinde eklenen kullanıcı sayısı gibi.

Bildirimleri işlemleri uygulayarak bir parçası olarak tetiklenir. Bu nedenle bildirimleri olası ve zaman uyumlu olayları pahalı işlemleri içermemelidir kadar hızlı ele alınmalıdır.

## <a name="reliable-state-manager-notifications"></a>Güvenilir durum Yöneticisi bildirimleri
Güvenilir durum Yöneticisi aşağıdaki olaylar için bildirimleri sağlar:

* İşlem
  * İşleme
* Durum Yöneticisi
  * Yeniden derleme
  * Güvenilir durum ekleme
  * Güvenilir durum temizleme

Güvenilir durum Yöneticisi geçerli hareket halindeki işlemleri izler. Yalnızca bir bildirimine harekete işlem durumu değişikliği işlenmeden bir işlemdir.

Güvenilir durum Yöneticisi gibi güvenilir bir sözlük ve güvenilir bir sıra güvenilir durumları koleksiyonunu tutar. Güvenilir durum Yöneticisi bu koleksiyonu değiştiğinde bildirim harekete: güvenilir durumu eklendiğinde veya kaldırıldığında ya da tüm koleksiyon yeniden oluşturulur.
Güvenilir durum Yöneticisi koleksiyonu üç durumlarda yeniden oluşturulur:

* Kurtarma: Bir çoğaltma başladığında, önceki durumuna diskten kurtarır. Kurtarma sonunda kullandığı **NotifyStateManagerChangedEventArgs** kurtarılan güvenilir durumları kümesini içeren bir olay harekete geçirmek için.
* Tam kopyalama: Bir çoğaltma yapılandırma kümesi katılabilmesi için önce oluşturulması gerekir. Bazı durumlarda, güvenilir durum Yöneticisi'nin durumu boşta ikincil çoğaltmaya uygulanması için birincil çoğaltmadan tam bir kopyasını gerektirir. İkincil Çoğaltmada güvenilir durum Yöneticisi kullanan **NotifyStateManagerChangedEventArgs** kullanarak birincil çoğaltmadan alınan güvenilir bildiren kümesini içeren bir olay harekete geçirmek için.
* Geri yükleme: Olağanüstü durum kurtarma senaryolarında yinelemenin durumu bir yedekten geri yüklenebilir **RestoreAsync**. Böyle durumlarda birincil çoğaltmadaki güvenilir durum Yöneticisi kullanan **NotifyStateManagerChangedEventArgs** yedekten geri güvenilir bildiren kümesini içeren bir olay harekete geçirmek için.

İşlem bildirimleri ve/veya durum manager bildirimlerine kaydetmek için ile kaydetmeniz gerekir **TransactionChanged** veya **StateManagerChanged** güvenilir durum Yöneticisi olayları. Oluşturucusu, durum bilgisi olan hizmet, bu olay işleyicileri ile kaydetmek için ortak bir yerdir. Oluşturucu kaydolduğunuzda, kullanım süresi boyunca bir değişiklik nedeniyle herhangi bir bildirim eksik olmaz **Telemetryclient**.

```csharp
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

**TransactionChanged** olay işleyicisi kullanır **NotifyTransactionChangedEventArgs** olay hakkındaki ayrıntıları sağlamak için. Action özelliği içerir (örneğin, **NotifyTransactionChangedAction.Commit**) değişiklik türünü belirtir. Ayrıca, değiştirilen işlem başvuru sağlayan işlem özelliği içerir.

> [!NOTE]
> Bugün, **TransactionChanged** işlem yalnızca, olayları ortaya çıkar. Eylemin ardından eşit olduğu **NotifyTransactionChangedAction.Commit**. Ancak gelecekte diğer tür durum değişikliklerini işlem için tetiklenen olayları. Eylemin denetleme ve yalnızca beklediğiniz bir ise, olay işleme öneririz.
> 
> 

Bir örnek aşağıdadır **TransactionChanged** olay işleyicisi.

```csharp
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

**StateManagerChanged** olay işleyicisi kullanır **NotifyStateManagerChangedEventArgs** olay hakkındaki ayrıntıları sağlamak için.
**NotifyStateManagerChangedEventArgs** iki alt sınıfları olmayan: **NotifyStateManagerRebuildEventArgs** ve **NotifyStateManagerSingleEntityChangedEventArgs**.
Eylem özelliğinde kullandığınız **NotifyStateManagerChangedEventArgs** dönüştürülecek **NotifyStateManagerChangedEventArgs** doğru alt Sınıflama:

* **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
* **NotifyStateManagerChangedAction.Add** ve **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Bir örnek aşağıdadır **StateManagerChanged** bildirim işleyici.

```csharp
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStateManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Güvenilir bir sözlük bildirimleri
Güvenilir bir sözlük aşağıdaki olaylar için bildirimleri sağlar:

* Yeniden oluştur: Çağrılır **ReliableDictionary** durumuna kurtarılan ya da kopyalanmış yerel durumu ya da yedekleme kurtarıldı.
* Temizle: Çağrılır durumunu **ReliableDictionary** aracılığıyla temizlenmiş **ClearAsync** yöntemi.
* Ekle: İçin bir öğe eklendiğinde çağırılır **ReliableDictionary**.
* Güncelleştirme: Bir öğe olduğunda çağrılır **IReliableDictionary** güncelleştirildi.
* Kaldır: Bir öğe olduğunda çağrılır **IReliableDictionary** silindi.

Güvenilir bir sözlük bildirimleri almak için kaydetmek gereken **DictionaryChanged** olay işleyicisini **IReliableDictionary**. Bu olay işleyicileri ile kaydetmek için ortak bir konum **ReliableStateManager.StateManagerChanged** bildirimi ekleyin.
Ne zaman kaydetme **IReliableDictionary** eklenir **Telemetryclient** herhangi bir bildirim eksik olmaz sağlar.

```csharp
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
        }
    }
}
```

> [!NOTE]
> **ProcessStateManagerSingleEntityNotification** örnek yöntemi olduğundan, önceki **OnStateManagerChangedHandler** örneği çağrıları.
> 
> 

Önceki kod kümeleri **IReliableNotificationAsyncCallback** , bunların ile arabirim **DictionaryChanged**. Çünkü **NotifyDictionaryRebuildEventArgs** içeren bir **IAsyncEnumerable** arabirimi--zaman uyumsuz olarak--sıralanması gerekir yeniden bildirimleri aracılığıyla tetiklenen  **RebuildNotificationAsyncCallback** yerine **OnDictionaryChangedHandler**.

```csharp
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

> [!NOTE]
> Önceki kodda, yeniden derleme bildirim işleme bir parçası olarak ilk işlenen toplam durum temizlenir. Güvenilir koleksiyon ile yeni bir durum yeniden olduğundan, tüm önceki bildirimler ilgisiz.
> 
> 

**DictionaryChanged** olay işleyicisi kullanır **NotifyDictionaryChangedEventArgs** olay hakkındaki ayrıntıları sağlamak için.
**NotifyDictionaryChangedEventArgs** beş alt sınıfları olmayan. Eylem özelliğini **NotifyDictionaryChangedEventArgs** dönüştürülecek **NotifyDictionaryChangedEventArgs** doğru alt Sınıflama:

* **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
* **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
* **NotifyDictionaryChangedAction.Add**: **NotifyDictionaryItemAddedEventArgs**
* **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
* **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```csharp
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Öneriler
* *Yapmak* bildirim olay mümkün olduğunca hızlı tamamlandı.
* *Sağlamadığı* pahalı işlemler (örneğin, g/ç işlemleri) zaman uyumlu olaylarının bir parçası olarak yürütün.
* *Yapmak* eylem türü olay işlemeden önce denetleyin. Yeni Eylem türleri gelecekte eklenebilir.

Akılda tutulması gereken bazı noktalar şunlardır:

* Bildirimler, bir işlemin yürütülmesi bir parçası olarak harekete geçirilir. Örneğin, bir geri bildirim, bir geri yükleme işleminin son adımı harekete geçirilir. Geri bildirim olayı işlenene kadar tamamlanmaz.
* Bildirimleri uygulama işlemlerinin bir parçası olarak başlatıldığından, istemciler yalnızca yerel olarak kaydedilmiş işlemleri için bildirimleri bakın. Ve işlemleri yalnızca yerel olarak kaydedilmiş olması garanti edilir (diğer bir deyişle, günlüğe), olabilir veya gelecekte geri olmayabilir.
* Yineleme yolu üzerinde uygulanan her işlem için tek bir uyarı tetiklenir. Başka bir deyişle, işlem T1 Create(X) Delete(X) ve Create(X) içeriyorsa, sırasıyla X, biri silme ve yeniden oluşturulması için bir tane oluşturulması için bir bildirim elde edersiniz.
* Birden çok işlem içeren işlemler için işlem kullanıcıdan birincil çoğaltmadaki alındıkları sırayla uygulanır.
* İşleme false ilerleme durumu bir parçası olarak, bazı işlemler geri olabilir. Bildirimler, çoğaltma durumunu kararlı bir noktaya geri alınıyor. Bu tür geri alma işlemleri için oluşturulur. Geri bildirimler önemli bir fark, yinelenen anahtarlara sahip olayları toplanır ' dir. Örneğin, T1 işlem geri alınamaz, Delete(X) için tek bir bildirim görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Koleksiyonlar](service-fabric-work-with-reliable-collections.md)
* [Reliable Services hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
* [Reliable Services yedekleme ve geri yükleme (olağanüstü durum kurtarma)](service-fabric-reliable-services-backup-restore.md)
* [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

