---
title: Azure Storage bloblarında (Önizleme) yumuşak Sil | Microsoft Docs
description: Böylece, yanlışlıkla değiştirilmiş veya bir uygulama veya başka bir depolama hesabı kullanıcı tarafından silinmiş olduğunda verilerinizi daha kolayca kurtarabilirsiniz azure Storage şimdi blob nesneler için geçici silme (Önizleme) sunar.
services: storage
author: MichaelHauss
manager: vamshik
ms.service: storage
ms.topic: article
ms.date: 03/21/2018
ms.author: mihauss
ms.openlocfilehash: 649838af1d4c753ac1d82a66c855ef313f14e85b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="soft-delete-for-azure-storage-blobs-preview"></a>Azure Storage bloblarında (Önizleme) için geçici silme

## <a name="overview"></a>Genel Bakış

Böylece, yanlışlıkla değiştirilmiş veya bir uygulama veya başka bir depolama hesabı kullanıcı tarafından silinmiş olduğunda verilerinizi daha kolayca kurtarabilirsiniz azure Storage şimdi blob nesneler için geçici silme (Önizleme) sunar.

## <a name="how-does-it-work"></a>Nasıl çalışır?

Etkinleştirildiğinde, geçici silme kaydetmek ve BLOB veya blob anlık görüntüleri silindiğinde verilerinizi kurtarmak sağlar. Bu koruma üzerine yazmaya sonucu olarak silinmeden blob verilerine genişletir.

Veri silindiğinde, kalıcı olarak silinmiş yerine geçici silinen durumuna geçiş yapar. Geçici silme açıktır ve verilerin üzerine üzerine veri durumunu kaydetmek için bir yumuşak silinen anlık görüntü oluşturulur. Geçici silinen nesneleri, açık bir şekilde listelenmiş sürece görünmez.
Kalıcı olarak süresi geçmeden önce geçici Silinen veriler kurtarılamaz süresini yapılandırabilirsiniz.

Geçici silme geriye dönük olarak uyumludur; Bu özellik ortaya koymaktadır korumaları yararlanmak için uygulamalarınız için değişiklik gerekmez. Ancak, [veri kurtarma](#recovery) yeni tanıtır **silmeyi geri al Blob** API.

> [!NOTE]
> Genel Önizleme sırasında anlık görüntüleri içeren bir blobu Blob katmanı ayarlamak çağrılması izin verilmiyor.
Geçici silme üzerine yazılır, verilerinizi korumak için anlık görüntü oluşturur. Etkin olarak anlık görüntüler içeren BLOB katmanlama etkinleştirmek için bir çözüm üzerinde çalışıyoruz.

### <a name="configuration-settings"></a>Yapılandırma ayarları

Yeni bir hesap oluşturduğunuzda, geçici silme varsayılan olarak kapalıdır. Geçici silme de var olan depolama hesapları için varsayılan olarak kapalıdır. Bir depolama hesabı kullanım ömrü boyunca herhangi bir zamanda açma ve kapatma özellik geçiş yapabilirsiniz.

Erişebilir ve özellik kapalıysa olduğunda özelliği daha önce açık olduğu zaman yazılım Silinen veriler kaydedilmiş varsayılarak yumuşak silinen verileri kurtarma. Üzerinde geçici silme açtığınızda, aynı zamanda saklama dönemi yapılandırmanız gerekir.

Saklama dönemi depolanan ve kurtarma için kullanılabilir yazılım Silinen veriler süreyi gösterir. Veri silindiğinde, BLOB'lar ve açıkça silinir blob anlık görüntüleri için Bekletme dönemi saati başlatır. Anlık görüntü oluşturulduğunda verilerin üzerine zaman geçici silme özelliği tarafından oluşturulan geçici silinen anlık görüntüleri, saatin başlatır. Şu an için geçici Silinen veriler 1 ile 365 gün arasında tutabilirsiniz.

Geçici silme saklama dönemi dilediğiniz zaman değiştirebilirsiniz. Güncelleştirilmiş saklama dönemi yalnızca yeni Silinen veriler için geçerlidir. Daha önce silinmiş veri tabanlı verilerin silindiğinde, yapılandırılan saklama dönemi sona erecek.

### <a name="saving-deleted-data"></a>Silinen verileri kaydetme

Geçici silme nerede BLOB'lar blob anlık görüntüleri silinmiş üzerine veya çoğu durumda, verilerinizi korur.

Kullanarak bir blob üzerine zaman **Put Blob**, **yerleştirme blok**, **Put engelleme listesi** veya **kopyalama Blob** öncesinde blob'un durumunun anlık görüntüsü yazma işlemi otomatik olarak oluşturulur. Bu anlık görüntü geçici silinen anlık görüntüsüdür; Geçici silinen nesneleri açıkça listelenen sürece görünmez durumdadır. Bkz: [kurtarma](#recovery) bölüm geçici silinen nesneleri listesinde öğrenin.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-overwrite.png)

*Etkin veri mavi modundayken yazılım silinen gri, verilerdir. Daha kısa süre önce yazılmış veriler eski verileri altında görünür. B0 B1 ile yazılır, geçici silinen anlık görüntüsünü B0 oluşturulur. B1 B2 ile yazılır, geçici silinen anlık görüntüsünü B1 oluşturulur.*

> [!NOTE]
> Geçici silme yalnızca ortaya koymaktadır hedef blob'un hesabı için açık olduğunda kopyalama işlemleri için koruma üzerine yazın.

> [!NOTE]
> Geçici silme göze değil arşiv katmanındaki bloblar için koruma üzerine yazın. Herhangi bir katmanı yeni bir blob'a ile bir blob arşivde üzerine yazılırsa üzerine blob kalıcı olarak süresi doldu.

Zaman **silmek Blob** çağrılır üzerinde anlık görüntü, bu anlık görüntü olarak yumuşak işaretlenmiş silindi. Yeni bir anlık görüntü oluşturulmaz.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-delete-snapshot.png)

*Etkin veri mavi modundayken yazılım silinen gri, verilerdir. Daha kısa süre önce yazılmış veriler eski verileri altında görünür. Zaman **anlık görüntü Blob** adlı B0 bir anlık görüntüsü olur ve B1 ise blob etkin durumu. B0 anlık görüntü silindiğinde, geçici olarak işaretlenmiş silindi.*

Zaman **silmek Blob** (herhangi bir blob diğer bir deyişle kendisi bir anlık görüntü) temel bir blob üzerindeki adlı bu blob olarak geçici olarak işaretlenmiş silindi. Önceki davranış ile tutarlı çağırma **silmek Blob** etkin anlık görüntülere sahip bir blob üzerindeki bir hata döndürür. Çağırma **silmek Blob** geçici silinen anlık görüntüler bir blob üzerinde bir hata döndürmez. Geçici silme açıldığında bir blob ve tek bir işlem içinde kendi anlık görüntüleri hala silebilirsiniz. Bunun yapılması temel blob işaretler ve anlık görüntüleri geçici olarak silinmiş.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-include.png)

*Etkin veri mavi modundayken yazılım silinen gri, verilerdir. Daha kısa süre önce yazılmış veriler eski verileri altında görünür. Burada, bir **silmek Blob** B2 ve ilişkili tüm anlık görüntüleri silmek için çağrı yapılır. Etkin blob, B2 ve ilişkili tüm anlık görüntüleri olarak geçici olarak işaretlenmiş silindi.*

> [!NOTE]
> Geçici silinen blob üzerine yazma işlemi önce blob'un durumunun geçici silinen anlık görüntüsü otomatik olarak oluşturulur. Yeni blob üzerine blob katmanı devralır.

Geçici silme değil kapsayıcı ya da hesap siler durumlarda verilerinizi kaydedin ve ne zaman meta verileri blob ve blob özellikleri üzerine yazılır. Bir depolama hesabı hatalı silinmeye karşı korumak için Azure Kaynak Yöneticisi'ni kullanarak bir kilit yapılandırabilirsiniz. Lütfen Azure Resource Manager makalesine bakın [engelle beklenmeyen değişiklikler kilit kaynaklara](../../azure-resource-manager/resource-group-lock-resources.md) daha fazla bilgi için.

Aşağıdaki tabloda, geçici silme açıldığında beklenen bir davranış Ayrıntılar verilmiştir:

| REST API işlemi                                                                                                | Kaynak türü                 | Açıklama                                                                                                 | Davranışını değiştirme                                                                                                                                                                                                                                                                                                                                                   |
|-------------------------------------------------------------------------------------------------------------------|-------------------------------|-------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Silme](/rest/api/storagerp/StorageAccounts/Delete)              | Hesap                       | Tüm kapsayıcıları ve içerdiği BLOB'ları da dahil olmak üzere depolama hesabını siler.                           | Hiçbir değişiklik. Kapsayıcılar ve bloblar silinen hesabındaki kurtarılabilir değildir.                                                                                                                                                                                                                                                                                          |
| [Delete Container](/rest/api/storageservices/fileservices/delete-container)       | Kapsayıcı                     | İçerdiği tüm BLOB'ları da dahil olmak üzere bu kapsayıcı siler.                                                | Hiçbir değişiklik. Silinen kapsayıcıdaki blobları kurtarılabilir değildir.                                                                                                                                                                                                                                                                                                       |
| [Put Blob](/rest/api/storageservices/fileservices/put-blob)                       | Blok, ekleme ve sayfa Blobları | Yeni bir blob oluşturur veya mevcut bir blob bir kapsayıcı içindeki değiştirir                                          | Bir blob değiştirmek için kullanılan ise çağrı önce blob'un durumunun anlık görüntüsü otomatik olarak oluşturulur. Aynı türde (blok, ekleme veya sayfa) blob tarafından değiştirilir ve yalnızca, bu daha önce yazılım silinmiş bir blob için de geçerlidir. Farklı türde blob tarafından değiştirilir, tüm mevcut yazılım Silinen veriler kalıcı olarak sona erecektir. |
| [Delete Blob](/rest/api/storageservices/fileservices/delete-blob)                 | Blok, ekleme ve sayfa Blobları | Bir blob veya blob anlık görüntü silme işlemi için işaretler. Blob veya anlık görüntü çöp toplama sırasında daha sonra silindi | Bu anlık görüntüyü bir blob anlık görüntü olarak geçici olarak işaretlenmiş silme için kullandıysanız silindi. Bir blobu silmek için kullandıysanız, bu blob olarak geçici olarak işaretlenmiş silindi.                                                                                                                                                                                                                           |
| [BLOB kopyalama](/rest/api/storageservices/fileservices/copy-blob)                     | Blok, ekleme ve sayfa Blobları | Hedef blob aynı depolama hesabındaki veya başka bir depolama hesabı için bir kaynak blob kopyalar.       | Bir blob değiştirmek için kullanılan ise çağrı önce blob'un durumunun anlık görüntüsü otomatik olarak oluşturulur. Aynı türde (blok, ekleme veya sayfa) blob tarafından değiştirilir ve yalnızca, bu daha önce yazılım silinmiş bir blob için de geçerlidir. Farklı türde blob tarafından değiştirilir, tüm mevcut yazılım Silinen veriler kalıcı olarak sona erecektir. |
| [Blok yerleştirme](/rest/api/storageservices/put-block)                                  | Blok Blobları                   | Bir blok blobu bir parçası olarak kabul edilebilmesi için yeni bir blok oluşturur.                                                | Etkin bir blob için bir blok yürütmek için kullanılan değişiklik yoktur. Silinen geçici bir blob için bir blok tamamlamaya kullandıysanız, yeni blob oluşturulur ve bir anlık görüntü geçici silinen blob durumunu yakalamak için otomatik olarak oluşturulur.                                                                                                                      |
| [Engelleme listesi yerleştirme](/rest/api/storageservices/fileservices/put-block-list)           | Blok Blobları                   | Bir blob, blok blok blobu oluşturan kimlikleri kümesi belirterek kaydeder.                             | Bir blob değiştirmek için kullanılan ise çağrı önce blob'un durumunun anlık görüntüsü otomatik olarak oluşturulur. Bir blok blobu olduğu ve yalnızca, bu daha önce yazılım silinmiş bir blob için de geçerlidir. Farklı türde blob tarafından değiştirilir, tüm mevcut yazılım Silinen veriler kalıcı olarak sona erecektir.                                                |
| [Sayfa yerleştirin](/rest/api/storageservices/fileservices/put-page)                       | Sayfa Blobları                    | Bir sayfa aralığını bir sayfa blob'u yazar.                                                                     | Hiçbir değişiklik. Üzerine veya bu işlemi kullanarak temizlenmiş sayfa Blob verilerini kaydedilmez ve kurtarılabilir değil.                                                                                                                                                                                                                                                   |
| [Blok Ekle](/rest/api/storageservices/fileservices/append-block)               | Ekleme Blobları                  | Bir ekleme blobu sonuna bir veri bloğunun Yazar                                                         | Hiçbir değişiklik.                                                                                                                                                                                                                                                                                                                                                           |
| [Blob özelliklerini ayarlama](/rest/api/storageservices/fileservices/set-blob-properties) | Blok, ekleme ve sayfa Blobları | Bir blob için tanımlanan sistem özellikleri için değerleri ayarlar.                                                       | Hiçbir değişiklik. Üzerine blob özellikleri kurtarılabilir değil.                                                                                                                                                                                                                                                                                                          |
| [Set Blob Metadata](/rest/api/storageservices/fileservices/set-blob-metadata)     | Blok, ekleme ve sayfa Blobları | Kümeleri kullanıcı tanımlı meta verileri belirtilen blob olarak bir veya daha fazla ad-değer çiftleri.                          | Hiçbir değişiklik. Üzerine blob meta verileri kurtarılabilir değil.                                                                                                                                                                                                                                                                                                             |

"Put üzerine veya bir sayfa blob'u aralıklarını Temizle sayfasına" çağırma otomatik olarak anlık görüntüleri oluşturmaz olduğunu fark önemlidir. Virtual machine diskleri sayfa Blobları tarafından yedeklenir ve kullanmak **Put sayfa** veri yazmak için.

### <a name="recovery"></a>Kurtarma

Silinen veri kurtarmak kolaylaştırmak için yeni bir "Blob silmeyi geri al" API sunulmuştur. Üzerinde geçici silinen temel blob silmeyi geri al API çağırma yükler ve ilişkili tüm etkin olarak geçici silinen anlık görüntüler. Tüm geri yüklemeler üzerinde etkin bir temel blob silmeyi geri al API çağırma geçici silinen anlık görüntüleri etkin olarak ilişkilendirilmiş. Anlık görüntü etkin olarak geri yüklenir, kullanıcı tarafından oluşturulan anlık görüntüleri gibi görünürler; temel blob üzerine yazmaz.

Bir blob çağırabilirsiniz belirli bir yazılım silinen anlık görüntüye geri yüklemek için **silmeyi geri al Blob** temel blob üzerinde. Daha sonra anlık görüntü şimdi etkin blob kopyalayabilirsiniz. Ayrıca, anlık görüntü için yeni bir blob kopyalayabilirsiniz.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-recover.png)

*Etkin veri mavi modundayken yazılım silinen gri, verilerdir. Daha kısa süre önce yazılmış veriler eski verileri altında görünür. Burada, **silmeyi geri al Blob** B, böylece temel blob, B1 ve ilişkili tüm anlık görüntüler, buradan geri blob üzerindeki etkin olarak yalnızca B0 denir. İkinci adımda B0 temel blob kopyalanır. Bu kopyalama işlemi B1 geçici silinen görüntüsünü oluşturur.*

Geçici silinen BLOB'ları görüntülemek ve anlık görüntüleri blob için silinen verileri içerecek şekilde seçebileceğiniz **listesi BLOB'ları**. Yalnızca geçici silinen temel BLOB'ları görüntüleyin ya da geçici silinen blob anlık eklemek için seçebilirsiniz. Tüm yazılım Silinen veriler için verilerin yanı sıra verileri kalıcı olarak sona erecektir önceki gün sayısı ne zaman silindiğini zaman görüntüleyebilirsiniz.

### <a name="example"></a>Örnek

Yükler, üzerine yazar bir .NET betiği, anlık görüntüler, siler, konsol çıktısı aşağıda verilmiştir ve yazılım zaman "HelloWorld" adlı bir blobu silmek geri yüklemeler açıktır:

```bash
Upload:
- HelloWorld (is soft deleted: False, is snapshot: False)

Overwrite:
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Snapshot:
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Delete (including snapshots):
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: False)

Undelete:
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Copy a snapshot over the base blob:
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)
```

Bkz: [sonraki adımlar](#Next steps) Bu çıktı üretilen uygulama için bir işaretçi bölümü.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Tüm yazılım Silinen veriler etkin veri aynı hızda faturalandırılır. Yapılandırılan saklama döneminden sonra kalıcı olarak silinir veri için ücret alınmaz. Anlık görüntüler ve bunların nasıl ücretler tahakkuk daha ayrıntılı bilgi edinmek için lütfen bkz. [nasıl anlık görüntüleri ücretler tahakkuk anlama](storage-blob-snapshots.md).

Anlık görüntüler otomatik oluşturmayla ilgili işlemler için Fatura edilecek değil. İçin Fatura edilecek **silmeyi geri al Blob** işlemleri "Yazma işlemleri" oranı.

Genel olarak Azure Blob Storage fiyatları hakkında daha fazla ayrıntı için kullanıma [Azure Blob Depolama fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/blobs/).

Başlangıçta üzerinde geçici silme etkinleştirdiğinizde, daha iyi özellik faturanızı nasıl etkileyeceğini anlamak için bir küçük Bekletme dönemi kullanmanızı öneririz.

## <a name="quickstart"></a>Hızlı Başlangıç

### <a name="azure-portal"></a>Azure portalına
Geçici silme etkinleştirmek için gidin **geçici silme** altında seçeneği **Blob hizmeti**. Ardından **etkin** ve geçici Silinen veriler korumak istediğiniz gün sayısını girin.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-configuration.png)

Geçici silinen BLOB'ları görüntülemek için seçin **Göster silinmiş BLOB'lar** onay kutusu.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted.png)

Belirli bir blob geçici silinen anlık görüntüleri görüntülemek için blob seçip'i tıklatın **görüntülemek anlık görüntüleri**.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots.png)

Emin olun **Göster silinen anlık görüntüleri** onay kutusu seçilidir.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots-check.png)

Bir geçici silinen blob veya anlık görüntü tıkladığınızda, yeni blob özelliklere dikkat edin. Bunlar zaman Nesne silindi ve kaç gün blob veya blob anlık görüntü kadar kalan kalıcı olarak süresi gösterir. Geçici silinen nesnenin bir anlık görüntü değilse, da onu silmeyi geri al seçeneğine sahip olursunuz.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-properties.png)

Bir blob silinen ayrıca tüm ilişkili anlık görüntüleri geri olduğunu unutmayın. Etkin bir blob için geçici silinen anlık görüntü geri almak için blob seçin tıklayın ve **tüm anlık görüntüleri geri**.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-undelete-all-snapshots.png)

Bir blob'un anlık görüntüleri geri sonra tıklayabilirsiniz **Yükselt** böylece blob anlık görüntüye geri yükleme kök blob üzerinden bir anlık görüntü kopyalanamadı.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-promote-snapshot.png)

### <a name="powershell"></a>PowerShell
Geçici silme etkinleştirmek için bir blob istemcinin hizmet özellikleri güncelleştirin. Aşağıdaki örnek, bir abonelik hesaplarında alt kümeleri için geçici silme sağlar:

```powershell
Set-AzureRmContext -Subscription "<subscription-name>"
$MatchingAccounts = Get-AzureRMStorageAccount | where-object{$_.StorageAccountName -match "<matching-regex>"}
$MatchingAccounts | Enable-AzureStorageDeleteRetentionPolicy -RetentionDays 7
```

Yanlışlıkla silinen BLOB'ları kurtarmak için bu blobları üzerinde silmeyi geri al çağırın. Bu arama unutmayın **silmeyi geri al Blob**, hem de etkin ve geçici silinen BLOB'lar, ilişkili tüm yazılım silinen anlık görüntüleri olarak etkin geri. Aşağıdaki örnek, tüm yazılım silinir ve etkin bir kapsayıcıdaki blobları üzerinde silmeyi geri al çağırır:
```powershell
# Create a context by specifying storage account name and key
$ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

# Get the blobs in a given container and show their properties
$Blobs = Get-AzureStorageBlob -Container $StorageContainerName -Context $ctx -IncludeDeleted
$Blobs.ICloudBlob.Properties

# Undelete the blobs
$Blobs.ICloudBlob.Undelete()
```
### <a name="python-client-library"></a>Python istemci kitaplığı

Geçici silme etkinleştirmek için bir blob istemcinin hizmet özellikleri güncelleştirin:

```python
# Make the requisite imports
from azure.storage.blob import BlockBlobService
from azure.storage.common.models import DeleteRetentionPolicy

# Initialize a block blob service
block_blob_service = BlockBlobService(account_name='<enter your storage account name>', account_key='<enter your storage account key>')

# Set the blob client's service property settings to enable soft delete
block_blob_service.set_blob_service_properties(delete_retention_policy = DeleteRetentionPolicy(enabled = True, days = 7))
```

### <a name="net-client-library"></a>.NET istemci kitaplığı

Geçici silme etkinleştirmek için bir blob istemcinin hizmet özellikleri güncelleştirin:

```csharp
// Get the blob client’s service property settings
ServiceProperties serviceProperties = blobClient.GetServiceProperties();

// Configure soft delete
serviceProperties.DeleteRetentionPolicy.Enabled = true;
serviceProperties.DeleteRetentionPolicy.RetentionDays = RetentionDays;

// Set the blob client’s service property settings
blobClient.SetServiceProperties(serviceProperties);
```

Yanlışlıkla silinen BLOB'ları kurtarmak için bu blobları üzerinde silmeyi geri al çağırın.
Bu arama unutmayın **silmeyi geri al Blob**, hem de etkin ve geçici silinen BLOB'lar, ilişkili tüm yazılım silinen anlık görüntüleri olarak etkin geri. Aşağıdaki örnek, tüm yazılım silinir ve etkin bir kapsayıcıdaki blobları üzerinde silmeyi geri al çağırır:

```csharp
// Recover all blobs in a container
foreach (CloudBlob blob in container.ListBlobs(useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Deleted))
{
       await blob.UndeleteAsync();
}
```

Belirli blob sürüme kurtarmak için ilk çağrıda silmeyi geri al bir blob sonra kopyalayın istenen anlık görüntü blob. Aşağıdaki örnekte, en son oluşturulan anlık bir blok blobuna kurtarır:

```csharp
// Undelete
await blockBlob.UndeleteAsync();

// List all blobs and snapshots in the container prefixed by the blob name
IEnumerable<IListBlobItem> allBlobVersions = container.ListBlobs(
    prefix: blockBlob.Name, useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Snapshots);

// Restore the most recently generated snapshot to the active blob    
CloudBlockBlob copySource = allBlobVersions.First(version => ((CloudBlockBlob)version).IsSnapshot &&
    ((CloudBlockBlob)version).Name == blockBlob.Name) as CloudBlockBlob;
blockBlob.StartCopy(copySource);
```

## <a name="should-i-use-soft-delete"></a>Geçici silme kullanmalıyım?

Verilerinizin yanlışlıkla değiştirilmiş veya bir uygulama veya başka bir depolama hesabı kullanıcı tarafından silinmiş bir fırsat ise, üzerinde geçici silme kapatma öneririz.
Geçici silme veri koruma stratejisinin bir parçası olan ve yanlışlıkla veri kaybını önlemeye yardımcı olabilir.

## <a name="faq"></a>SSS

**Hangi depolama türleri için geçici silme kullanabilir miyim?**

Geçici silme şu anda yalnızca blob (nesne) depolama birimi olarak kullanılabilir.

**Geçici silme tüm depolama hesap türleri için kullanılabilir mi?**

Evet, geçici silme de blob storage hesapları için genel amaçlı depolama hesaplarındaki BLOB'lar için kullanılabilir. Bu, standart ve premium hesaplara uygulanır. Geçici silme yönetilen diskler için kullanılabilir değil.

**Geçici silme için tüm depolama katmanları var mı?**

Evet, geçici silme tüm etkin, etkin olmayan dahil olmak üzere depolama katmanları ve Arşiv için kullanılabilir. Ancak, geçici silme değil göze arşiv katmanındaki bloblar için koruma üzerine yazın.

**Premium depolama hesaplarına sahip bir blob anlık görüntü sınırı olan 100 başına. Geçici silinen anlık görüntüleri bu sınırında sayılır mı?**

Hayır, yumuşak silinen anlık görüntüleri bu sınırında saymak değil.

**Var olan depolama hesapları için geçici silme üzerinde kapatabilir miyim?**

Evet, geçici silme mevcut ve yeni depolama hesapları için yapılandırılabilir.

**Tüm hesap veya kapsayıcı açık geçici silme ile silersem, tüm ilişkili BLOB'lar kaydedilecek?**

Hayır, tüm hesap veya kapsayıcı silerseniz, tüm ilişkili BLOB'lar kalıcı olarak silinecek. Bir depolama hesabını yanlışlıkla silmeleri korumak öğrenmek için Azure Resource Manager makalesine bakın [engelle beklenmeyen değişiklikler kilit kaynaklara](/azure-resource-manager/resource-group-lock-resources.md).

**Silinen veriler için kapasite ölçümleri görüntüleyebilir miyim?**

Geçici Silinen veriler toplam depolama hesabı kapasitesi bir parçası olarak dahil edilir. İzleme ve depolama kapasitesini izleme hakkında daha fazla bilgi için lütfen bkz [depolama çözümlemeleri](../common/storage-analytics.md) makalesi.

**Geçici silme devre dışı bıraktığınızda ı hala geçici silinen verilere erişmek erişebilecek mi?**

Evet, bu erişebilir ve geçici silme devre dışı bırakıldığında süresi dolmamış geçici silinen veri kurtarma devam edersiniz.

**I okuyabilir ve geçici silinen anlık görüntülerini my blob kopyalama?**

Evet, ancak silmeyi geri al blob üzerindeki ilk çağırmanız gerekir.

**Tüm türleri blob için geçici silme var mı?**

Evet, geçici silme blok Blobları, ekleme Blobları ve sayfa BLOB'ları için kullanılabilir.

**Geçici silme için sanal makine disklerini var mı?**

Geçici silme premium ve standart yönetilmeyen diskler için kullanılabilir. Geçici silme yalnızca yardımcı olacak tarafından silinen veriler kurtarma **silmek Blob**, **Put Blob**, **Put engelleme listesi**, **yerleştirme blok** ve **Kopyalama Blob**. Bir çağrı tarafından üzerine veri **Put sayfa** kurtarılabilir değil.

**Varolan uygulamalarım geçici silme kullanmak için değiştirmeniz gerekiyor mu?**

Kullanmakta olduğunuz API sürümü ne olursa olsun geçici silme yararlanmak mümkündür. Ancak, liste ve geçici silinen BLOB'lar ve blob anlık görüntüleri kurtarmak için 2017 07 29 sürümünü kullanmanız gerekir [Storage Services REST API](https://docs.microsoft.com/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) veya daha büyük. Genel olarak, her zaman bu özellik kullanıp bağımsız olarak en son sürümünü kullanmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar

* [.NET örnek kod](https://github.com/Azure-Samples/storage-dotnet-blob-soft-delete)

* [BLOB hizmeti REST API'si](/rest/api/storageservices/fileservices/blob-service-rest-api)

* [Azure Storage çoğaltma](../common/storage-redundancy.md)

* [Yüksek oranda kullanılabilir RA-GRS kullanarak uygulamalar tasarlama](../common/storage-designing-ha-apps-with-ragrs.md)

* [Bir Azure Storage kesinti oluşursa yapmanız gerekenler](../common/storage-disaster-recovery-guidance.md)
