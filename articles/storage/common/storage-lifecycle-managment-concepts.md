---
title: Azure depolama yaşam döngüsü yönetimi
description: Seyrek erişimli ve Arşiv katmanlarını erişimliden geçiş againg veri yaşam döngüsü ilkesi kuralları oluşturmayı öğrenin.
services: storage
author: yzheng-msft
manager: jwillis
ms.service: storage
ms.workload: storage
ms.topic: article
ms.date: 04/30/2018
ms.author: yzheng
ms.openlocfilehash: bd36cfd0cd03592396a2aa9a977124880f47ec90
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248478"
---
# <a name="managing-the-azure-blob-storage-lifecycle-preview"></a>Azure Blob Depolama yaşam döngüsü (Önizleme) yönetme

Veri kümeleri benzersiz yaşam döngüleri vardır. Bazı veriler genellikle erken çevriminin erişilen ancak erişimine gerek büyük ölçüde veri yaş bırakır. Bazı veriler bulutta boşta kalır ve bir kez depolanan nadiren erişilir. Diğer veri kümelerini etkin olarak okuma ve kendi ömürleri değiştiren bazı veriler gün veya ay oluşturulduktan sonra süresi dolacak. Azure Blob Storage yaşam döngüsü yönetimi (Önizleme) verilerinizi en iyi erişim katmanı için geçiş yapmak ve verileri kendi yaşam döngüsünün sonuna süresi dolmak üzere kullanabileceğiniz zengin, kurala dayalı bir ilke sunar.

Yaşam döngüsü yönetimi ilkesi yardımcı olur:

- Geçiş için depolama katmanına, etkin arşivi, seyrek erişimli veya arşive seyrek erişimli (etkin) BLOB'ları performans ve maliyet için en iyi duruma getirme
- BLOB'ları kendi ömürleri sonunda silme
- (GPv2 ve Blob Depolama hesaplarını destekler) depolama hesabı düzeyinde günde bir kez çalıştırılacak kurallarını tanımlama
- Kapsayıcı ya da bir alt kümesini (önekleri filtre olarak kullanarak) BLOB'ları kuralları uygula

Yaşam döngüsü erken aşaması sırasında sık erişilen, yalnızca gerekli veri kümesini göz önünde bulundurun bazen iki hafta sonra ve bir ay sonra ve sonrasında nadiren erişilir. Bu senaryoda, sık erişimli depolama erken aşamaları sırasında en iyisidir, seyrek erişimli depolama arada sırada erişim için en uygun olan ve Arşiv depolama en iyi katman veri yaş sonra bir ay seçenektir. Depolama katmanları verilerin yaşını açısından ayarlayarak, gereksinimleriniz için en ucuz depolama seçenekleri tasarlayabilirsiniz. Bu geçiş elde etmek için yaşam döngüsü yönetimi ilkeleri için katmanlara eskime verileri taşımak kullanılabilir.

## <a name="storage-account-support"></a>Depolama hesabı desteği

Yaşam Döngüsü Yönetimi İlkesi ile hem genel amaçlı v2 kullanılabilir (GPv2) hesabı ve Blob Depolama hesabı. Azure portalında basit bir tek tıklamayla işlem aracılığıyla GPv2 hesabına, varolan genel amaçlı (GPv1) hesabı dönüştürebilirsiniz. Daha fazla bilgi için bkz. [Azure depolama hesabı seçenekleri](../common/storage-account-options.md).  

## <a name="pricing"></a>Fiyatlandırma 

Yaşam döngüsü yönetimi Önizleme'de ücretsiz özelliğidir. Müşteriler, normal işlem maliyetini ücretlendirilirsiniz [listesi BLOB'ları](https://docs.microsoft.com/rest/api/storageservices/list-blobs) ve [ayarlamak Blob katmanı](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) API çağrıları. Bkz: [blok blobu fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/blobs/) fiyatlandırma hakkında daha fazla bilgi edinmek için.

## <a name="register-for-preview"></a>Önizleme için kaydolun 
Genel önizlemede kaydetmek için bu özellik, aboneliğinizde kaydetmek için bir istek göndermeniz gerekir. (Birkaç gün içinde) isteğiniz onaylandıktan sonra tüm mevcut ve yeni GPv2 veya Blob Storage hesabında Batı ABD 2 ve Batı Orta ABD özelliğinin etkin gerekir. Önizleme sırasında yalnızca blok blob desteklenir. İST ulaşana kadar ile çoğu önizlemeleri gibi bu özelliği üretim iş yükleri için kullanılmamalıdır

Bir istek göndermek için aşağıdaki PowerShell veya CLI komutları çalıştırın.

### <a name="powershell"></a>PowerShell

Bir istek göndermek için:

```powershell
Register-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage 
```
Aşağıdaki komutla kayıt onay durumunu kontrol edebilirsiniz:
```powershell
Get-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage
```
Özellik onaylanmış ve uygun şekilde kaydedildiğini, "Kaydedildi" durumu almanız gerekir.

### <a name="cli-20"></a>CLI 2.0

Bir istek göndermek için: 
```cli
az feature register –-namespace Microsoft.Storage –-name DLM
```
Aşağıdaki komutla kayıt onay durumunu kontrol edebilirsiniz:
```cli
-az feature show –-namespace Microsoft.Storage –-name DLM
```
Özellik onaylanmış ve uygun şekilde kaydedildiğini, "Kaydedildi" durumu almanız gerekir. 


## <a name="add-or-remove-policies"></a>İlkeleri Ekle Kaldır 

Ekleme, düzenleme veya Azure portalını kullanarak bir ilke kaldırma [PowerShell](https://www.powershellgallery.com/packages/AzureRM.Storage/5.0.3-preview), REST API'leri veya istemci araçları aşağıdaki dillerde: [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/8.0.0-preview), [Python](https://pypi.org/project/azure-mgmt-storage/2.0.0rc3/), [ Node.js]( https://www.npmjs.com/package/azure-arm-storage/v/5.0.0), [Ruby]( https://rubygems.org/gems/azure_mgmt_storage/versions/0.16.2). 

### <a name="azure-portal"></a>Azure portalına

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesabınıza gitmek için Tüm Kaynaklar’ı ve ardından depolama hesabınızı seçin.

3. Ayarlar dikey penceresinde tıklayın **yaşam döngüsü yönetimi** görüntülemek ve/veya ilkelerini değiştirmek için Blob hizmeti altında gruplandırılır.

### <a name="powershell"></a>PowerShell

```powershell
$rules = '{ ... }' 

Set-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName] -Policy $rules 

Get-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName]
```

> [!NOTE]
Depolama hesabınız için güvenlik duvarı kurallarını etkinleştirirseniz, yaşam döngüsü yönetimi istekleri engellenmiş olabilir. Özel durumlar sağlayarak engellemesini kaldırabilirsiniz. Daha fazla bilgi için özel durumlar kısmına bakın [güvenlik duvarları ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

## <a name="policies"></a>İlkeler

Yaşam döngüsü yönetimi ilkesi, bir JSON belgesi kurallarında koleksiyonudur:

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


Bir ilke içinde iki parametreler gereklidir:

| Parametre adı | Parametre türü | Notlar |
|----------------|----------------|-------|
| sürüm        | Bir dize olarak ifade edilen `x.x` | 0,5 Önizleme sürüm numarasıdır |
| rules          | Kural nesnelerinin bir dizisi | En az bir kuralı her bir ilkede gereklidir. Önizleme sırasında ilke başına en fazla 4 kuralları belirtebilirsiniz. |

İçindeki bir kural Gerekli Parametreler şunlardır:

| Parametre adı | Parametre türü | Notlar |
|----------------|----------------|-------|
| Ad           | Dize | Bir kural adı, alfasayısal karakterlerin herhangi bir birleşimini içerebilir. Kural adı büyük/küçük harf duyarlıdır. Bir ilke içinde benzersiz olmalıdır. |
| type           | Enum değeri | Önizleme için geçerli bir değerdir `Lifecycle` |
| tanım     | Yaşam döngüsü kuralı tanımlayan bir nesne | Her tanımı, bir filtre kümesi ve bir eylem kümesi ile yapılır. |

## <a name="rules"></a>Kurallar

Her kural tanımı bir filtre kümesi ve bir eylem kümesi içerir. Aşağıdaki örnek kural temel blok blobları önekiyle katmanını değiştirir `foo`. İlkede, bu kuralları olarak tanımlanmıştır:

- 30 gün sonra son değiştirilme seyrek erişimli depolama katmanı blob
- Arşiv depolama blobu 90 gün sonra son değiştirilme katmanı
- BLOB 2,555 gün (7 yıl) son değişikliğinden sonra silin.
- 90 gün sonra anlık görüntü oluşturma BLOB anlık görüntüleri silin

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
          "prefixMatch": [ "foo" ]
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

Filtreler BLOB Depolama hesabındaki bir alt kural eylemi sınırlayın. Birden çok filtre tanımlanmışsa, mantıksal `AND` tüm filtreleri gerçekleştirilir.

Önizleme sırasında geçerli filtreler aşağıdakileri içerir:

| Filtre adı | Filtre türü | Notlar | Gereklidir |
|-------------|-------------|-------|-------------|
| blobTypes   | Önceden tanımlanmış enum değerleri dizisi. | Yalnızca önizleme sürümü için `blockBlob` desteklenir. | Evet |
| prefixMatch | Olması eşleşecek şekilde önekler için bir dizeler dizisi. | Tanımlı değil, bu kural hesaptaki tüm BLOB'lar için geçerlidir. | Hayır |

### <a name="rule-actions"></a>Kural eylemi

Yürütme koşul karşılandığında Eylemler filtrelenmiş BLOB'larını uygulanır.

Önizleme'de, yaşam döngüsü yönetimi katmanlama ve blob silinmesini ve blob anlık görüntüleri silinmesini destekler. Her kural en az bir eylemin BLOB veya blob anlık görüntüleri tanımlanmış olması gerekir.

| Eylem        | Temel Blob                                   | Anlık Görüntü      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Şu anda etkin katmanı, BLOB'ları destekler         | Desteklenmiyor |
| tierToArchive | Şu anda etkin veya etkin olmayan katmanı, BLOB'ları destekler | Desteklenmiyor |
| sil        | Desteklenen                                   | Desteklenen     |

>[!NOTE] 
Birden çok eylem aynı blob üzerindeki tanımlanmışsa yaşam döngüsü yönetimi için blob ucuz eylemini uygular. (örn., eylem `delete` eyleminden daha ucuz olan `tierToArchive`. Eylem `tierToArchive` eyleminden daha ucuz olan `tierToCool`.)

Önizleme'de, eylem yürütme koşullar yaş üzerinde temel alır. Son değiştirme zamanı yaş izlemek ve yaş izlemek için anlık görüntüleri kullanan anlık görüntü oluşturma zaman blob için temel blob kullanır.

| Eylem yürütme koşulu | Koşul değeri | Açıklama |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Gün cinsinden yaş gösteren tamsayı değeri | Temel blob eylemler için geçerli bir koşul |
| daysAfterCreationGreaterThan     | Gün cinsinden yaş gösteren tamsayı değeri | Blob anlık eylemler için geçerli bir koşul | 

## <a name="examples"></a>Örnekler
Aşağıdaki örnekler, yaygın senaryolar yaşam döngüsü ilkesi kurallarına adres göstermektedir.

### <a name="move-aging-data-to-a-cooler-tier"></a>Eskime veri için bir katmanına taşıyın

Aşağıdaki örnek önekine sahip blok blobları geçiş yapmayı gösteren `foo` veya `bar`. İlke seyrek erişimli depolama için 30 gün içinde değişiklik yapılmamış henüz BLOB'ları ve Arşiv katmanına 90 gün içinde değişiklik yapılmamış BLOB'lar geçişler:

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
            "prefixMatch": [ "foo", "bar" ]
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

### <a name="archive-data-at-ingest"></a>Arşiv verileri alma 

Bazı veriler bulutta boşta kalır ve depolandıktan sonra, olursa, nadiren erişilir. Bu veriler hemen alınan sonra arşivlenecek en iyisidir. Aşağıdaki yaşam döngüsü ilkesi alma verileri arşivlemek için yapılandırılır. Bu örnek blok blobları öneki ile depolama hesabındaki geçişleri `archive` hemen içine bir arşiv katmanı. Hemen geçiş 0 gün sonra son değişiklik zamanını BLOB'ları üzerinde hareket tarafından gerçekleştirilir:

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
            "prefixMatch": [ "archive" ]
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

Bazı verileri, gün veya ay maliyetlerini azaltmak veya kamu yasal düzenlemelerle uyumlu oluşturulduktan sonra süresi dolacak şekilde beklenir. Süresi dolacak şekilde yaşam döngüsü yönetimi ilkesi ayarlanabilir silme tarihte tabanlı veri yaş üzerinde. Aşağıdaki örnekte tüm blok blobları (ile belirtilen önek) silen bir ilke 365 günden eski gösterir.

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

Değiştirilebilir ve düzenli olarak yaşam süresi boyunca erişilen veriler için anlık görüntü genellikle eski sürümleri verileri izlemek için kullanılır. Anlık görüntü yaş üzerinde göre eski anlık görüntüleri siler bir ilke oluşturabilirsiniz. Anlık görüntü yaşı, anlık görüntü oluşturma zaman değerlendirerek belirlenir. Bu ilke kuralı siler engelleme blob anlık görüntüleri önekiyle `activeData` 90 gün olan veya anlık görüntü oluşturulduktan sonra eski.

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
            "prefixMatch": [ "activeData" ]
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

## <a name="next-steps"></a>Sonraki adımlar

Yanlışlıkla silindikten sonra verileri kurtarmak öğrenin:

- [Azure Storage BLOB'ları için yumuşak silme ](../blobs/storage-blob-soft-delete.md)
