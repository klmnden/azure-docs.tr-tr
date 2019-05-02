---
title: Azure depolama blobları için geçici silme | Microsoft Docs
description: Azure Storage şimdi blob nesneler için geçici silme sunar, böylece, yanlışlıkla değişiklik veya bir uygulama veya başka bir depolama hesabı kullanıcı tarafından silinmiş verilerinizi daha kolay geri yükleyebilirsiniz.
services: storage
author: MichaelHauss
ms.service: storage
ms.topic: article
ms.date: 04/23/2019
ms.author: mihauss
ms.subservice: blobs
ms.openlocfilehash: d9055b0c0decbeca0bb43969af4e854c396c3bb6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64727425"
---
# <a name="soft-delete-for-azure-storage-blobs"></a>Azure depolama BLOB'ları için geçici silme
Azure Storage şimdi blob nesneler için geçici silme sunar, böylece, yanlışlıkla değişiklik veya bir uygulama veya başka bir depolama hesabı kullanıcı tarafından silinmiş verilerinizi daha kolay geri yükleyebilirsiniz.

## <a name="how-does-it-work"></a>Nasıl çalışır?
Etkinleştirildiğinde, geçici silme kaydedin ve BLOB'ları veya blob anlık görüntüleri silindiğinde verilerinizi kurtarmanıza olanak sağlar. Bu koruma, üzerine yazma sonucu olarak silinmesi blob verilerine genişletir.

Veri silindiğinde, kalıcı olarak silinmiş yerine geçici silinen duruma geçer. Geçici silme açıktır ve verilerin üzerine verilerin üzerine yazılması durumunu kaydetmek için geçici silinen bir anlık görüntü üretilir. Geçici silinen nesneleri açıkça listelenen sürece görünmez. Geçici olarak silinen verileri kalıcı olarak süresinin dolması kurtarılabilir süresini yapılandırabilirsiniz.

Geçici silme geriye dönük uyumludur; Bu özellik ortaya koymaktadır korumaları yararlanmak için uygulamalarınızı herhangi bir değişiklik gerekmez. Ancak, [veri kurtarma](#recovery) yeni kullanıma sunuyor **Blob silme işlemi geri** API.

### <a name="configuration-settings"></a>Yapılandırma ayarları
Yeni bir hesap oluşturduğunuzda, geçici silme varsayılan olarak kapalıdır. Geçici silme ayrıca var olan depolama hesapları için varsayılan olarak kapalıdır. Bir depolama hesabı kullanım ömrü boyunca herhangi bir zamanda açıp özellik geçiş yapabilirsiniz.

Erişebilir ve bu özelliği devre dışı olduğunda geçici olarak silinen verileri kurtarmak varsayarak özelliği daha önce açıldı, geçici olarak silinen verileri kaydedildi. Geçici silme üzerinde etkinleştirdiğinizde, ayrıca saklama süresi yapılandırmanız gerekir.

Saklama dönemi, geçici olarak silinen verileri depolanan ve kurtarma için kullanılabilir olduğu süre miktarını gösterir. Veriler silindiğinde blobları ve açıkça silinir blob anlık görüntüleri için bekletme süresini başlatır saat. Anlık görüntü oluşturulduğunda verilerin üzerine zaman geçici silme özelliği tarafından oluşturulan geçici silinen anlık görüntüleri için saati başlar. Şu anda 1-365 gün arasında için geçici olarak silinen verileri koruyabilirsiniz.

Geçici silme saklama süresini istediğiniz zaman değiştirebilirsiniz. Güncelleştirilmiş Bekletme dönemi yalnızca yeni Silinen veriler için geçerlidir. Önceden silinen veriler göre verileri silindiğinde yapılandırdığınız saklama süresi dolacak. Geçici olarak silinmiş bir nesneyi silmeye çalışırsanız, süre sonu etkilemez.

### <a name="saving-deleted-data"></a>Silinen verileri kaydetme
Geçici silme burada bloblar veya blob anlık görüntüleri üzerine veya silinirse, çoğu durumda, verilerinizi korur.

Kullanarak bir blobun üzerine yazıldığında **Put Blob**, **blok yerleştirme**, **Put Engellenenler listesi** veya **kopya blob'u** öncesinde blobun durumunun bir anlık görüntüsü yazma işlemi otomatik olarak oluşturulur. Bu anlık görüntü geçici silinen bir anlık görüntü, Geçici silinen nesneleri açıkça listelenen sürece görünmez durumdadır. Bkz: [kurtarma](#recovery) bölümü geçici silinen nesneleri listesinde öğrenin.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-overwrite.png)

*Etkin veri mavi modundayken geçici olarak silinen verileri gri, olur. Daha kısa süre önce yazılmış veri eski verileri altında görünür. Geçici silinen anlık görüntüsünü B0 B0 ile B1 üzerine yazıldığında oluşturulur. Geçici silinen anlık görüntüsünü B1, B1 B2 ile üzerine yazıldığında oluşturulur.*

> [!NOTE]  
> Geçici silme yalnızca ortaya koymaktadır hedef blob hesabı için açıldığı zaman kopyalama işlemleri için koruma üzerine yaz.

> [!NOTE]  
> Geçici silme göze değil arşiv katmanındaki bloblar için koruma üzerine yazın. Yeni bir blob içinde herhangi bir katmanı ile bir blob arşiv üzerine yazıldıysa üzerine blob kalıcı olarak süresi doldu.

Zaman **Blob silme** çağrılır, anlık görüntü üzerinde anlık görüntü, geçici olarak işaretlenmiş silindi. Yeni bir anlık görüntü oluşturulmadı.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-delete-snapshot.png)

*Etkin veri mavi modundayken geçici olarak silinen verileri gri, olur. Daha kısa süre önce yazılmış veri eski verileri altında görünür. Zaman **anlık görüntü Blob** adlı B0 bir anlık görüntü haline gelir ve B1 ise blob etkin durumu. B0 anlık görüntüsü silinir, geçici olarak işaretlenmiş silindi.*

Zaman **Blob silme** (tüm blob diğer bir deyişle kendisi bir anlık görüntü) temel blob üzerinde çağrılır, blob olarak geçici olarak işaretlenmiş silindi. Önceki davranışı ile tutarlı çağırma **Blob silme** etkin anlık görüntülerine sahip bir blob üzerinde bir hata döndürür. Çağırma **Blob silme** geçici silinen anlık görüntüleri olan üzerinde bir hata döndürmez. Geçici silme açık olduğunda, bir blob ve tek işlemde tüm anlık görüntüleri yine de silebilirsiniz. Bunun yapılması temel blob işaretler ve anlık görüntüleri geçici olarak silinmiş.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-include.png)

*Etkin veri mavi modundayken geçici olarak silinen verileri gri, olur. Daha kısa süre önce yazılmış veri eski verileri altında görünür. Burada bir **Blob silme** B2 ve ilişkili tüm anlık görüntüleri silmek için çağrı yapılır. Etkin blob, B2 ve ilişkili tüm anlık görüntüleri olarak geçici olarak işaretlenmiş silindi.*

> [!NOTE]  
> Geçici silinen bir blob üzerine yazma işlemi öncesinde blobun durumu geçici silinen anlık görüntüsünü otomatik olarak oluşturulur. Yeni blob, üzerine blobun katmanını devralır.

Geçici silme değil kapsayıcı ya da hesabı silme durumunda verilerinizi kaydetmek ya da zaman meta verileri blob ve blob özelliklerini üzerine yazılır. Bir depolama hesabı hatalı silinmeye karşı korumak için Azure Resource Manager kullanarak kilit yapılandırabilirsiniz. Lütfen Azure Resource Manager makalesine bakın [beklenmeyen değişiklikleri önlemek için kaynakları kilitleme](../../azure-resource-manager/resource-group-lock-resources.md) daha fazla bilgi için.

Geçici silme açıldığında Beklenen davranış aşağıdaki tabloda Ayrıntılar:

| REST API işlemi | Kaynak türü | Açıklama | Davranışını değiştirme |
|--------------------|---------------|-------------|--------------------|
| [Silme](/rest/api/storagerp/StorageAccounts/Delete) | Hesap | Tüm kapsayıcı ve içerdiği blobların dahil, bir depolama hesabını siler.                           | Değişiklik yok. Kapsayıcılar ve bloblar silinen hesabı olarak kurtarılabilir değildir. |
| [Kapsayıcıyı Sil](/rest/api/storageservices/delete-container) | Kapsayıcı | İçerdiği tüm blobları dahil olmak üzere kapsayıcı siler. | Değişiklik yok. Silinen kapsayıcıdaki blobları kurtarılabilir değildir. |
| [Put Blob](/rest/api/storageservices/put-blob) | Bloğu ekleme ve sayfa Blobları | Yeni bir blob oluşturur veya mevcut bir bloba bir kapsayıcı içindeki değiştirir | Mevcut bir bloba değiştirmek için kullanılan çağrıdan önce blobun durumunun bir anlık görüntüsünü otomatik olarak oluşturulur. Blob (blok, ekleme veya sayfa) aynı tür tarafından değiştirilir ve yalnızca, bu daha önce geçici silinen blob için de geçerlidir. Farklı türde bir blob tarafından değiştirilirse, tüm var olan geçici olarak silinen verileri kalıcı olarak sona erecektir. |
| [Delete Blob](/rest/api/storageservices/delete-blob) | Bloğu ekleme ve sayfa Blobları | Bir blobu veya blob anlık görüntüsü silme işlemi için işaretler. Çöp toplama sırasında blob veya anlık görüntü daha sonra silinir | Blob anlık görüntüsü, bu anlık görüntü olarak geçici olarak işaretlenmiş silme için kullandıysanız silindi. Bir blobun silinmesi için kullandıysanız, bu blob olarak geçici olarak işaretlenmiş silindi. |
| [Kopya blob'u](/rest/api/storageservices/copy-blob) | Bloğu ekleme ve sayfa Blobları | Kaynak blob bir hedef blob aynı depolama hesabındaki veya başka bir depolama hesabına kopyalar. | Mevcut bir bloba değiştirmek için kullanılan çağrıdan önce blobun durumunun bir anlık görüntüsünü otomatik olarak oluşturulur. Blob (blok, ekleme veya sayfa) aynı tür tarafından değiştirilir ve yalnızca, bu daha önce geçici silinen blob için de geçerlidir. Farklı türde bir blob tarafından değiştirilirse, tüm var olan geçici olarak silinen verileri kalıcı olarak sona erecektir. |
| [Blok yerleştirme](/rest/api/storageservices/put-block) | Blok Blobları | Bir blok blobu türünde bir parçası olarak kabul edilebilmesi için yeni bir blok oluşturur. | Etkin olan bir blob için bir blok uygulamak için kullanılan bir değişiklik yoktur. Geçici silinen bir blob için bir blok tamamlamaya kullandıysanız, yeni bir blob oluşturulur ve bir anlık görüntü geçici silinen blob durumunu yakalamak için otomatik olarak oluşturulur. |
| [Engelleme listesi yerleştirin](/rest/api/storageservices/put-block-list) | Blok Blobları | Bir blobun blok blok blobu oluşturan kimlikleri kümesi belirterek kaydeder. | Mevcut bir bloba değiştirmek için kullanılan çağrıdan önce blobun durumunun bir anlık görüntüsünü otomatik olarak oluşturulur. Bir blok blobu olduğu ve yalnızca, bu daha önce geçici silinen blob için de geçerlidir. Farklı türde bir blob tarafından değiştirilirse, tüm var olan geçici olarak silinen verileri kalıcı olarak sona erecektir. |
| [Sayfa yerleştirin](/rest/api/storageservices/put-page) | Sayfa Blobları | Sayfa aralığı bir sayfa Blob olarak yazar. | Değişiklik yok. Üzerine yazılır veya bu işlemi kullanarak seçili sayfa blobu veri kaydedilmez ve kurtarılamaz. |
| [Blok ekleme](/rest/api/storageservices/append-block) | Ekleme Blobları | Bir ekleme blobu sonuna bir veri bloğu yazma | Değişiklik yok. |
| [Blob özelliklerini ayarlama](/rest/api/storageservices/set-blob-properties) | Bloğu ekleme ve sayfa Blobları | Bir blob için tanımlanan sistem özellikleri için değerleri ayarlar. | Değişiklik yok. Üzerine blob özelliklerini kurtarılabilir değildir. |
| [BLOB meta verileri ayarlama](/rest/api/storageservices/set-blob-metadata) | Bloğu ekleme ve sayfa Blobları | Kümeleri kullanıcı tanımlı meta veriler bir veya daha fazla ad-değer çiftleri olarak belirtilen blob. | Değişiklik yok. Üzerine blob meta verilerini kurtarılabilir değil. |

"Sayfası üzerine veya bir sayfa blob'u aralıklarını temizlemek için Put" çağırma otomatik anlık görüntüleri oluşturmaz, dikkat edin önemlidir. Sanal makine diskleri sayfa Blobları tarafından yedeklenir ve kullanmak **Put sayfası** veri yazmak için.

### <a name="recovery"></a>Kurtarma
Silinen verileri kurtarmak kolay hale getirmek için yeni bir "Blob silmeyi geri al" API ekledik. Geçici silinen temel blob üzerinde silmeyi geri alma API'sini çağırma yükler ve ilişkili tüm etkin olarak geçici silinen anlık görüntüleri. Silmeyi geri alma API'sini, tüm geri yüklemeler üzerinde etkin bir temel blob çağırma geçici silinen anlık görüntüleri etkin olarak ilişkilendirilmiş. Etkin olarak anlık görüntü geri yüklendiğinde, kullanıcı tarafından oluşturulan anlık görüntüleri gibi görünürler; temel blobun üzerine yazmayın.

Bir blob çağırabilirsiniz belirli bir geçici silinen anlık görüntüye geri yüklemek için **Blob silme işlemi geri** temel blob üzerinde. Ardından, şimdi etkin blobu anlık görüntü kopyalayabilirsiniz. Ayrıca, anlık görüntü için yeni bir blob kopyalayabilirsiniz.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-recover.png)

*Etkin veri mavi modundayken geçici olarak silinen verileri gri, olur. Daha kısa süre önce yazılmış veri eski verileri altında görünür. Burada, **Blob silme işlemi geri** B, böylece temel blob, B1 ve ilişkili tüm anlık görüntüler, burada geri blob üzerinde etkin olarak yalnızca B0 çağrılır. İkinci adımda, temel blobu B0 kopyalanır. Bu kopyalama işlemi, B1 geçici silinen anlık görüntüsünü oluşturur.*

Geçici silinen blobları görüntüleyin ve blob anlık görüntüleri silinen verilerin dahil edileceği seçebileceğiniz **Blobları listeleme**. Yalnızca geçici silinen temel bloblarını görüntülemek veya geçici silinen bir blob anlık görüntülerini de içerecek şekilde seçebilirsiniz. Tüm geçici olarak silinen verileri için verileri kalıcı olarak süresi dolacak önce geçen gün sayısını yanı sıra veri ne zaman silinmiş zaman görüntüleyebilirsiniz.

### <a name="example"></a>Örnek
Yükler, üzerine yazar .NET betiğini, anlık görüntüler, siler, konsol çıktısı verilmiştir ve geçici olduğunda "HelloWorld" adlı bir blobun silinmesi geri yüklemeler açıktır:

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

Bkz: [sonraki adımlar](#next-steps) bölümü bu çıkışı üreten uygulamaya işaretçisi.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama
Tüm geçici olarak silinen verileri etkin veri olarak aynı oranda faturalandırılır. Yapılandırılmış saklama döneminden sonra kalıcı olarak silinen verileri için ücretlendirilmez. Anlık görüntüler ve bunların nasıl ücretler tahakkuk daha ayrıntılı bilgi edinmek için lütfen bkz [nasıl anlık görüntüleri ücretler tahakkuk anlama](storage-blob-snapshots.md).

Otomatik anlık görüntüleri ile ilgili işlemler için faturalandırılmazsınız. İçin faturalandırılırsınız **Blob silme işlemi geri** işlemler "Yazma işlemleri" fiyatı.

Genel olarak Azure Blob Depolama fiyatlarında daha fazla ayrıntı için kullanıma [Azure Blob Depolama fiyatlandırması sayfasını](https://azure.microsoft.com/pricing/details/storage/blobs/).

Geçici silme üzerinde ilk kez açtığınızda, bir küçük saklama süresi özelliği faturanızı nasıl etkileyeceğini daha iyi anlamak için kullanmanızı öneririz.

## <a name="quickstart"></a>Hızlı Başlangıç
### <a name="azure-portal"></a>Azure portal
Geçici silme etkinleştirmek için gidin **geçici silme** altındaki **Blob hizmeti**. Ardından **etkin** ve geçici olarak silinen verileri elde tutmak istediğiniz gün sayısını girin.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-configuration.png)

Geçici silinen blobları görüntülemek için seçin **silinen blobları Göster** onay kutusu.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted.png)

Belirli bir blobun geçici silinen anlık görüntülerini görüntülemek için blobu seçin sonra **anlık görüntüleri görüntüle**.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots.png)

Emin **silinen anlık görüntüleri Göster** onay kutusunun seçili.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots-check.png)

Bir geçici silinen bir blob veya anlık görüntüsünü tıkladığınızda, yeni blob özelliklere dikkat edin. Bunlar, nesne silindikten sonra ve blob veya blob anlık görüntüsü kadar kaç gün kaldığını kalıcı olarak süresi gösterir. Geçici silinen nesnenin bir anlık görüntü değilse, da, silmeyi geri alma seçeneğine sahip olursunuz.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-properties.png)

Silinen bir blob ayrıca tüm ilişkili anlık görüntüyü silme işlemi olduğunu unutmayın. Etkin bir blob için geçici silinen anlık görüntüyü silme işlemi için blob seçin tıklayın ve **tüm anlık görüntüleri silmeyi geri alma**.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-undelete-all-snapshots.png)

Bir blobun anlık görüntüyü silme işlemi sonra tıklayabilirsiniz **Yükselt** böylece blob anlık görüntüye geri yükleme kök blob üzerinde anlık görüntü kopyalanacak.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-promote-snapshot.png)

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Geçici silme etkinleştirmek için bir blob istemcinin hizmet özellikleri güncelleştirin. Aşağıdaki örnek, bir Abonelikteki hesapların bir alt kümesi için geçici silme sağlar:

```powershell
Set-AzContext -Subscription "<subscription-name>"
$MatchingAccounts = Get-AzStorageAccount | where-object{$_.StorageAccountName -match "<matching-regex>"}
$MatchingAccounts | Enable-AzStorageDeleteRetentionPolicy -RetentionDays 7
```
Aşağıdaki komutu kullanarak bu geçici silme açıldı doğrulayabilirsiniz:

```powershell
$MatchingAccounts | Get-AzStorageServiceProperty -ServiceType Blob
```

Yanlışlıkla silinen blobları kurtarmak için bu bloblarda silmeyi geri al'ı çağırın. Çağırmanın unutmayın **Blob silme işlemi geri**, hem de etkin ve geçici silinen blobları etkin olarak ilişkili tüm geçici silinen anlık geri. Aşağıdaki örnek bir kapsayıcıdaki tüm geçici silinen ve etkin blobları silme işlemini geri alma çağırır:
```powershell
# Create a context by specifying storage account name and key
$ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

# Get the blobs in a given container and show their properties
$Blobs = Get-AzStorageBlob -Container $StorageContainerName -Context $ctx -IncludeDeleted
$Blobs.ICloudBlob.Properties

# Undelete the blobs
$Blobs.ICloudBlob.Undelete()
```
Geçerli geçici silme bekletme ilkesini bulmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
   $account = Get-AzStorageAccount -ResourceGroupName myresourcegroup -Name storageaccount
   Get-AzStorageServiceProperty -ServiceType Blob -Context $account.Context
```

### <a name="azure-cli"></a>Azure CLI 
Geçici silme etkinleştirmek için bir blob istemcinin hizmet özellikleri güncelleştirin:

```azurecli-interactive
az storage blob service-properties delete-policy update --days-retained 7  --account-name mystorageaccount --enable true
```

Yumuşak doğrulamak için delete açıktır, aşağıdaki komutu kullanın: 

```azurecli-interactive
az storage blob service-properties delete-policy show --account-name mystorageaccount 
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
// Get the blob client's service property settings
ServiceProperties serviceProperties = blobClient.GetServiceProperties();

// Configure soft delete
serviceProperties.DeleteRetentionPolicy.Enabled = true;
serviceProperties.DeleteRetentionPolicy.RetentionDays = RetentionDays;

// Set the blob client's service property settings
blobClient.SetServiceProperties(serviceProperties);
```

Yanlışlıkla silinen blobları kurtarmak için bu bloblarda silmeyi geri al'ı çağırın. Çağırmanın unutmayın **Blob silme işlemi geri**, hem de etkin ve geçici silinen blobları etkin olarak ilişkili tüm geçici silinen anlık geri. Aşağıdaki örnek bir kapsayıcıdaki tüm geçici silinen ve etkin blobları silme işlemini geri alma çağırır:

```csharp
// Recover all blobs in a container
foreach (CloudBlob blob in container.ListBlobs(useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Deleted))
{
       await blob.UndeleteAsync();
}
```

Bir bloba sürüme kurtarmak için blob üzerinde silme işlemini geri alma çağırın, sonra istenen anlık görüntü blobu kopyalayın. Aşağıdaki örnek, en son oluşturulan anlık görüntüsü bir blok blobuna kurtarır:

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
Verilerinizi yanlışlıkla değişiklik veya bir uygulama veya başka bir depolama hesabı kullanıcı tarafından silinmiş bir fırsat varsa, üzerinde geçici silme kapatma öneririz. Geçici silme bir veri koruma stratejisinin bir parçasıdır ve yanlışlıkla veri kaybını önlemeye yardımcı olabilir.

## <a name="faq"></a>SSS
**Geçici silmeyi kullanma tüm özel durumlar var mı?**  
Etkinleştirme geçici silme sık üzerine yazılan veriler için daha yüksek depolama kapasitesi ücretler ve daha yüksek gecikme süresiyle blobları listelerken neden olabilir. Bu, ayrı bir depolama hesabında geçici silme ile devre dışı sık üzerine veri depolayarak azaltabilirsiniz. 

**Geçici silme için hangi depolama türlerini kullanabilirim?**  
Geçici silme şu anda yalnızca blob (nesne) depolama olarak kullanılabilir.

**Geçici silme tüm depolama hesap türleri için kullanılabilir mi?**  
Evet, geçici silme de blob depolama hesapları bloblar genel amaçlı, olduğu gibi kullanılabilir depolama hesaplarını (GPv1 ve GPv2). Bu, standart ve premium hesaplarına uygulanır. Geçici silme, yönetilen diskler için kullanılamıyor.

**Geçici silme tüm depolama katmanları için kullanılabilir mi?**  
Evet, tüm depolama katmanları dahil olmak üzere sık erişimli, seyrek erişimli ve Arşiv geçici silme kullanılabilir. Geçici silme değil ancak göze arşiv katmanındaki bloblar için koruma üzerine yazın.

**Geçici silinen anlık görüntüleri ile BLOB katmanı için Blob katmanı API kümesini kullanabilir miyim?**  
Evet. Geçici silinen anlık görüntüleri özgün katmanda kalır, ancak temel blob yeni katmanına taşınacaktır. 

**Premium depolama hesaplarına sahip bir blob anlık görüntü sınırı 100 başına. Geçici silinen anlık görüntüleri, bu sınırında sayılır mı?**  
Hayır, geçici olarak silinen anlık görüntüleri bu sınırında sayısı değil.

**Mevcut depolama hesapları için geçici silme üzerinde kapatabilir miyim?**  
Evet, mevcut ve yeni depolama hesapları için geçici silme yapılandırılabilirdir.

**Tüm ilişkili blobları, tüm hesap ya da kapsayıcı açık olarak geçici silme ile silersem, kaydedilecek?**  
Hayır, tüm hesap ya da kapsayıcı silerseniz, tüm ilişkili blobları kalıcı olarak silinecek. Bir depolama hesabını yanlışlıkla silmeleri koruma konusunda bilgi edinmek için lütfen Azure Resource Manager makalesine bakın [beklenmeyen değişiklikleri önlemek için kaynakları kilitleme](../../azure-resource-manager/resource-group-lock-resources.md).

**Silinen veriler için kapasite ölçümlerini görüntüleyebilirim?**  
Geçici olarak silinen verileri, toplam depolama hesabı kapasitesi bir parçası olarak dahil edilir. İzleme ve depolama kapasitesi izleme hakkında daha fazla bilgi için lütfen bkz [depolama analizi](../common/storage-analytics.md) makalesi.

**Geçici silme açarsam miyim geçici olarak silinen verileri erişmeye devam edebilir?**  
Evet, erişebilir ve geçici silmeyi devre dışı bırakıldığında, süresi dolmamış geçici olarak silinen verileri kurtarma devam edersiniz.

**Okuma ve miyim geçici silinen anlık görüntülerini my blob kopyalama?**  
Evet, ancak ilk bloba Undelete çağırmalıdır.

**Tüm türleri blob için geçici silme kullanılabilir mi?**  
Evet, geçici silme blok Blobları, ekleme Blobları ve sayfa Blobları için kullanılabilir.

**Geçici silme, sanal makine diskleri için kullanılabilir mi?**  
Geçici silme, premium ve standart yönetilmeyen diskler için kullanılabilir. Geçici silme yalnızca yardımcı olacak olarak silinen verileri kurtarmak **Blob silme**, **Put Blob**, **Put Engellenenler listesi**, **blok yerleştirme** ve **Blob kopyalama**. Bir çağrı tarafından üzerine veri **Put sayfa** kurtarılamaz.

**Geçici silme kullanmak için mevcut uygulamalarımı değiştirmem gerekiyor mu?**  
Kullanmakta olduğunuz API sürümünden bağımsız olarak geçici silme yararlanmak mümkündür. Ancak, liste ve geçici silinen blobları ve blob anlık görüntüleri kurtarmak için 2017-07-29 sürümünü kullanmanız gerekir [Storage Services REST API](https://docs.microsoft.com/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) veya büyük. Genel olarak, her zaman bu özelliği kullanmanıza bakılmaksızın en son sürümünü kullanmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
* [.NET örnek kodu](https://github.com/Azure-Samples/storage-dotnet-blob-soft-delete)
* [BLOB hizmeti REST API'si](/rest/api/storageservices/blob-service-rest-api)
* [Azure depolama çoğaltma](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [RA-GRS'yi kullanarak yüksek kullanılabilirliğe sahip uygulamalar tasarlama](../common/storage-designing-ha-apps-with-ragrs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Olağanüstü durum kurtarma ve depolama hesabı yük devretme (Önizleme) Azure Depolama'daki](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
