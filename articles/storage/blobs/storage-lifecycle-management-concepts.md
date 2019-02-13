---
title: Azure depolama yaşam döngüsünü yönetme
description: Yaşam döngüsü ilkesi kuralları geçiş eskime verileri seyrek erişimli ve Arşiv katmanları için sık erişimli'den oluşturmayı öğrenin.
services: storage
author: yzheng-msft
ms.service: storage
ms.topic: article
ms.date: 11/04/2018
ms.author: yzheng
ms.subservice: common
ms.openlocfilehash: 284a590a484052fdb7da2f03c6155078268b2aac
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56211453"
---
# <a name="managing-the-azure-blob-storage-lifecycle-preview"></a>Azure Blob Depolama (Önizleme) yaşam döngüsünü yönetme

Veri kümeleri benzersiz yaşam döngüleri vardır. Yaşam döngüsünde kişiler bazı veriler genellikle erişebilir. Ancak veriler eskidikçe önemli ölçüde erişim gereksinimini bırakır. Bazı veriler bulutta boşta kalır ve saklanmaya başlanan nadiren erişilir. Başka veri Setleri etkin bir şekilde okuyun ve kendi ömürleri değiştirilmiş olsa bazı veriler gün veya ay, oluşturulduktan sonra süresi dolar. Azure Blob Depolama yaşam döngüsü yönetimi (Önizleme), GPv2 ve Blob Depolama hesapları için zengin, kural tabanlı bir ilke sunar. Verilerinizi uygun erişim katmanları için geçiş ya da veri yaşam döngüsü sonunda sona ilkesini kullanın.

Yaşam döngüsü yönetim ilkesini sağlar:

- Geçiş bir depolama katmana (seyrek erişimli, arşivlemek için sık erişimli veya arşiv-seyrek erişimli sık erişimli) için BLOB'ları performansı ve maliyet açısından en iyi duruma getirme
- Döngülerini sonunda bloblarını silin
- Depolama hesabı düzeyinde günde bir kez çalıştırılacak kurallar tanımlama
- Kapsayıcıları ve blobları (ön ekleri filtre olarak kullanarak) bir alt kümesi için kuralları uygula

Burada bir veri kümesi sık erişim erken yaşam döngüsünün aşamaları boyunca ancak ardından nadiren iki hafta sonra alır senaryoyu ele alalım. İlk aydan sonra veri kümesi nadiren erişilir. Bu senaryoda, sık erişimli depolama erken aşamalarında en iyisidir. Seyrek erişimli depolama, arada sırada erişim için en uygun ve Arşiv depolama en iyi katmanı veri yaş sonra bir aydan uzun bir seçenektir. Depolama katmanları verilerin yaşını açısından ayarlayarak, ihtiyaçlarınız için en uygun maliyetli depolama seçenekleri tasarlayabilirsiniz. Bu geçiş elde etmek için yaşam döngüsü yönetim ilkesi kurallarını eskime verileri harika katmanlara taşımak kullanılabilir.

## <a name="storage-account-support"></a>Depolama hesabı desteği

Yaşam döngüsü yönetim ilkesi hem genel amaçlı v2 ile kullanılabilir (GPv2) hesapları ve Blob Depolama hesapları. Azure portalında genel amaçlı (GPv1) hesabınız, basit bir tek tıklama işlemiyle aracılığıyla GPv2 hesabına yükseltebilirsiniz. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).  

## <a name="pricing"></a>Fiyatlandırma 

Yaşam döngüsü yönetimi ücretsiz önizleme olarak özelliğidir. Müşteriler normal işlem maliyetini ücretlendirilir [Blobları listeleme](https://docs.microsoft.com/rest/api/storageservices/list-blobs) ve [Blob katmanını ayarlama](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) API çağrıları. Fiyatlandırma hakkında daha fazla bilgi için bkz. [blok blobu fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="register-for-preview"></a>Önizleme için kaydolun 
Genel önizlemeye kaydolmak için aboneliğinize bu özelliği kaydetmek için bir istek göndermeniz gerekir. 72 saat içinde istek genellikle onaylanmış. Onay sonrasında, tüm mevcut ve yeni GPv2 veya Blob Depolama hesapları şu bölgelerde özellikler şunları içerir: Batı ABD 2, Batı Orta ABD, Doğu ABD 2 ve Batı Avrupa. Önizleme, yalnızca blok blob destekler. Tarife ulaşana kadar gibi çoğu Önizleme ile bu özelliği üretim iş yükleri için kullanmamanız gerekir

Bir istek göndermek için aşağıdaki PowerShell veya CLI komutları çalıştırın.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bir istek göndermek için:

```powershell
Register-AzProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage 
```
Aşağıdaki komutla kayıt onay durumunu kontrol edebilirsiniz:
```powershell
Get-AzProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage
```
Onay ve uygun kayıt ile aldığınız *kayıtlı* önceki isteklerde ne zaman gönderdiğiniz belirtin.

### <a name="azure-cli"></a>Azure CLI

Bir istek göndermek için: 
```cli
az feature register --namespace Microsoft.Storage --name DLM
```
Aşağıdaki komutla kayıt onay durumunu kontrol edebilirsiniz:
```cli
az feature show --namespace Microsoft.Storage --name DLM
```
Onay ve uygun kayıt ile aldığınız *kayıtlı* önceki isteklerde ne zaman gönderdiğiniz belirtin.


## <a name="add-or-remove-a-policy"></a>Bir ilke ekleyip 

Ekleme, düzenleme veya Azure portalını kullanarak bir ilkeyi kaldırdığınızda [PowerShell](https://www.powershellgallery.com/packages/Az.Storage), [Azure CLI](https://docs.microsoft.com/cli/azure/ext/storage-preview/storage/account/management-policy?view=azure-cli-latest#ext-storage-preview-az-storage-account-management-policy-create), [REST API'leri](https://docs.microsoft.com/rest/api/storagerp/managementpolicies/createorupdate), ya da istemci araçları aşağıdaki dillerde: [.NET ](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/8.0.0-preview), [Python](https://pypi.org/project/azure-mgmt-storage/2.0.0rc3/), [Node.js]( https://www.npmjs.com/package/azure-arm-storage/v/5.0.0), [Ruby](https://rubygems.org/gems/azure_mgmt_storage/versions/0.16.2). 

### <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Seçin **tüm kaynakları** ve ardından depolama hesabınızı seçin.

3. Seçin **yaşam döngüsü yönetimi (Önizleme)** görüntülemek veya değiştirmek ilkeniz için Blob hizmeti altında gruplandırılır.

### <a name="powershell"></a>PowerShell

```powershell
$rules = '{ ... }' 

Set-AzStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName] -Policy $rules 

Get-AzStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName]
```

### <a name="azure-cli"></a>Azure CLI

```
az account set --subscription "[subscriptionName]”
az extension add --name storage-preview
az storage account management-policy show --resource-group [resourceGroupName] --account-name [accountName]
```

> [!NOTE]
Depolama hesabınız için güvenlik duvarı kuralları etkinleştirirseniz, yaşam döngüsü yönetimi istekleri engellenebilir. Bu istekler, özel durumlar sağlayarak engelini kaldırabilirsiniz. Daha fazla bilgi için bkz: özel durumlar [güvenlik duvarları ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

## <a name="policy"></a>İlke

Yaşam döngüsü yönetim ilkesi kuralları bir JSON belgesinde koleksiyonudur:

```json
{
  "version": "0.5",
  "rules": [
    {
      "name": "rule1",
      "type": "Lifecycle",
      "definition": {...}
    },
    {
      "name": "rule2",
      "type": "Lifecycle",
      "definition": {...}
    }
  ]
}
```


Bir ilke, iki parametre gerektirir:

| Parametre adı | Parametre türü | Notlar |
|----------------|----------------|-------|
| version        | Bir dize olarak ifade edilen `x.x` | Önizleme sürümü 0,5 sayısıdır. |
| rules          | Kural nesnelerinin bir dizisi | Her bir ilkede en az bir kural gerekir. Önizleme sırasında ilke başına en fazla 4 kuralları belirtebilirsiniz. |

Her bir kural ilke içinde üç parametreler gereklidir:

| Parametre adı | Parametre türü | Notlar |
|----------------|----------------|-------|
| Ad           | String | Kural adı, alfasayısal karakterlerin herhangi bir birleşimini içerebilir. Kural adı büyük/küçük harf duyarlıdır. Bir ilke içinde benzersiz olmalıdır. |
| type           | Bir sabit listesi değeri | Geçerli Önizleme değeri `Lifecycle`. |
| tanım     | Yaşam döngüsü kuralı tanımlayan bir nesne | Her tanım, bir filtre kümesi ve bir eylem kümesinden oluşur. |

## <a name="rules"></a>Kurallar

Her kural tanımı bir filtre kümesi ve bir eylem kümesi içerir. [Filtre kümesi](#rule-filters) adları nesneleri veya bir kapsayıcı içindeki nesneler belirli bir dizi kural eylemi sınırlar. [Eylem kümesi](#rule-actions) katmanı uygular veya filtrelenmiş nesne kümesini eylemleri sil.

### <a name="sample-rule"></a>Örnek kural
Aşağıdaki örnek kural yalnızca eylemleri çalıştırmak için hesap filtreleri `container1/foo`. İçinde mevcut olan tüm nesneleri için aşağıdaki eylemleri çalıştırmak `container1` **ve** ile başlayan `foo`: 

- 30 gün sonra son değiştirilme katmanını seyrek erişimli blob katmanı
- Katmanı son değiştirilme 90 gündür arşiv katmanında blob
- BLOB, son değiştirme sonrasında 2,555 gün (yedi yıl) Sil
- Anlık görüntü oluşturulduktan sonra 90 gün BLOB anlık görüntüleri silin

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "ruleFoo", 
      "type": "Lifecycle", 
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}

```

### <a name="rule-filters"></a>Kural filtreleri

Filtreler, BLOB Depolama hesabında bir alt kural eylemi sınırlayın. Birden fazla filtre tanımlanırsa, bir mantıksal `AND` tüm filtreleri çalıştırır.

Önizleme süresince geçerli filtreler aşağıdakileri içerir:

| Filtre adı | Filtre türü | Notlar | Gereklidir |
|-------------|-------------|-------|-------------|
| blobTypes   | Önceden tanımlanmış bir sabit listesi değerleri dizisi. | Önizleme sürümü yalnızca destekler `blockBlob`. | Evet |
| prefixMatch | Olması eşleşecek şekilde ön ekleri için dize dizisi. Bir önek dizesi, bir kapsayıcı adı ile başlamalıdır. Örneğin, tüm BLOB'ları altındaki eşleştirmek istiyorsanız "https://myaccount.blob.core.windows.net/container1/foo/..." için bir kural, prefixMatch olduğu `container1/foo`. | PrefixMatch tanımlamazsanız, hesabındaki tüm bloblar için kurallar uygulanır. | Hayır |

### <a name="rule-actions"></a>Kural eylemi

Yürütme koşul karşılandığında eylemler için filtrelenmiş bloblara uygulanır.

Önizleme'de, katmanlama ve silme BLOB ve blob anlık görüntülerin silinmesi, yaşam döngüsü yönetimini destekler. BLOB'ları veya blob anlık görüntüleri, her kural için en az bir eylem tanımlayın.

| Eylem        | Temel Blob                                   | Anlık Görüntü      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Sık erişimli katmanı şu anda bloblarını destekler         | Desteklenmiyor |
| tierToArchive | Seyrek veya sık erişimli katmanı şu anda bloblarını destekler | Desteklenmiyor |
| delete        | Desteklenen                                   | Desteklenen     |

>[!NOTE] 
Aynı bloba birden fazla eylem tanımlarsanız, yaşam döngüsü yönetimi için blob ucuz eylemini uygular. Örneğin, eylem `delete` eyleminden daha ucuz `tierToArchive`. Eylem `tierToArchive` eyleminden daha ucuz `tierToCool`.

Önizleme aşamasında olan eylem yürütme koşullar üzerinde yaş temel alır. Temel blobları yaş izlemek için son değiştirilme zamanı kullanın ve yaş izlemek için anlık görüntüleri kullanmak anlık görüntü oluşturma zamanı blob.

| Eylem yürütme durumu | Koşul değeri | Açıklama |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Yaş gün cinsinden belirten bir tamsayı değeri | Temel blob eylemler için geçerli bir koşul |
| daysAfterCreationGreaterThan     | Yaş gün cinsinden belirten bir tamsayı değeri | Blob anlık görüntüsü eylemler için geçerli bir koşul | 

## <a name="examples"></a>Örnekler
Aşağıdaki örnekler, yaygın senaryolarda yaşam döngüsü ilkesi kuralları ile nasıl ekleyebileceğiniz gösterilmektedir.

### <a name="move-aging-data-to-a-cooler-tier"></a>Bir katmana eskime veri taşıma

Bu örnekte ön ekine sahip blok blobları geçiş gösterilmektedir `container1/foo` veya `container2/bar`. İlke, 30 günden önce seyrek erişimli depolamaya içinde değiştirilmesini BLOB'ları ve Arşiv katmanına 90 gün içinde değiştirilmemiş blobları geçişleri:

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "agingRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "container1/foo", "container2/bar" ]
          },
          "actions": {
            "baseBlob": {
              "tierToCool": { "daysAfterModificationGreaterThan": 30 },
              "tierToArchive": { "daysAfterModificationGreaterThan": 90 }
            }
          }
        }      
    }
  ]
}
```

### <a name="archive-data-at-ingest"></a>Arşiv veri alma 

Bazı veriler bulutta boşta kalır ve nadiren istenir; her zamankinden çok kez eriştiyseniz depolanır. Alındıktan sonra hemen bu verileri arşivleme. Aşağıdaki yaşam döngüsü ilkesi alma, verileri arşivlemek için yapılandırılır. Bu örnekte geçişleri blok blobları depolama hesabında kapsayıcı içindeki `archivecontainer` hemen içine bir arşiv katmanı. Hemen geçiş bloblarda 0 gün sonra son değiştirilme zamanı işlevi tarafından gerçekleştirilir:

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "archiveRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "archivecontainer" ]
          },
          "actions": {
            "baseBlob": { 
                "tierToArchive": { "daysAfterModificationGreaterThan": 0 }
            }
          }
        }      
    }
  ]
}

```

### <a name="expire-data-based-on-age"></a>Üzerinde yaş göre verileri süresi dolacak

Bazı veriler, gün veya ay maliyetleri düşürmeyi veya kamu gereksinimlerini karşılamak için oluşturulduktan sonra süresi dolacak şekilde beklenmektedir. Bir yaşam döngüsü yönetim ilkesi tarafından veri yaş üzerinde temel silme işlemi verileri süresi dolacak şekilde yapılandırabilirsiniz. Aşağıdaki örnek, tüm blok BLOB'ları (ile belirtilen önek) silen bir ilke 365 günden eski gösterir.

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "expirationRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ]
          },
          "actions": {
            "baseBlob": {
              "delete": { "daysAfterModificationGreaterThan": 365 }
            }
          }
        }      
    }
  ]
}
```

### <a name="delete-old-snapshots"></a>Eski anlık görüntüleri silin

Değiştirilebilir ve yaşam süresi boyunca düzenli olarak erişilen veriler için anlık görüntüleri genellikle daha eski sürümleri veri izlemek için kullanılır. Eski anlık görüntüleri üzerinde anlık görüntü yaş tabanlı silen bir ilke oluşturabilirsiniz. Anlık görüntü yaş değerlendirme anlık görüntü oluşturma zamanı tarafından belirlenir. Bu ilke kuralı silmeleri engelleme kapsayıcıdaki blob anlık görüntüleri `activedata` 90 gün olan veya eski anlık görüntü oluşturulduktan sonra.

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "snapshotRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "activedata" ]
          },
          "actions": {            
            "snapshot": {
              "delete": { "daysAfterCreationGreaterThan": 90 }
            }
          }
        }      
    }
  ]
}
```
## <a name="faq---i-created-a-new-policy-why-are-the-actions-not-run-immediately"></a>SSS - Eylemler neden hemen çalıştırılmaz, yeni bir ilke oluşturduğum? 

Platform yaşam döngüsü ilkesi günde bir kez çalışır. Yeni bir ilke ayarladıktan sonra Başlat ve Çalıştır bazı eylemler (örneğin, katmanlama ve silme) için 24 saate kadar sürebilir.  

## <a name="next-steps"></a>Sonraki adımlar

Yanlışlıkla silme işleminden sonra veri kurtarma işlemleri gerçekleştirmeyi öğreneceksiniz:

- [Azure depolama BLOB'ları için geçici silme ](../blobs/storage-blob-soft-delete.md)
