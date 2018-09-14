---
title: Azure depolama yaşam döngüsünü yönetme
description: Sık erişimli-seyrek erişimli ve Arşiv katmanları geçiş againg veri yaşam döngüsü ilkesi kuralları oluşturmayı öğrenin.
services: storage
author: yzheng-msft
ms.service: storage
ms.topic: article
ms.date: 04/30/2018
ms.author: yzheng
ms.component: common
ms.openlocfilehash: edc0cf7c8700e1ab728109bc9cbfda8135a59ee9
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574751"
---
# <a name="managing-the-azure-blob-storage-lifecycle-preview"></a>Azure Blob Depolama yaşam döngüsü (Önizleme) yönetme

Veri kümeleri benzersiz yaşam döngüleri vardır. Bazı verilere yaşam döngüsünde başlarında sık erişilebilir, ancak erişim için gereken önemli ölçüde veri yaş bırakır. Bazı veriler bulutta boşta kalır ve saklanmaya başlanan nadiren erişilir. Başka veri Setleri etkin bir şekilde okuyun ve kendi ömürleri değiştirilmiş olsa bazı veriler gün veya ay oluşturulduktan sonra süresi dolar. Azure Blob Depolama yaşam döngüsü yönetimi (Önizleme), verilerinizi en iyi erişim katmanına geçiş yapmak ve veri yaşam döngüsü sonunda süresi dolacak şekilde kullanabileceğiniz, kurala dayalı zengin bir ilke sunar.

Yaşam döngüsü yönetim ilkesini size yardımcı olur:

- Geçiş harika bir depolama katmanına (sık, seyrek erişimliden arşive, sık erişimli veya arşiv için seyrek erişimli olarak) BLOB'ları performansı ve maliyet açısından en iyi duruma getirme
- Döngülerini sonunda bloblarını silin
- (Hem GPv2 ve Blob Depolama hesaplarını destekler) depolama hesabı düzeyinde günde bir kez yürütülecek kurallar tanımlama
- Kapsayıcıları ve blobları (ön ekleri filtre olarak kullanarak) bir alt kümesi için kuralları uygula

Bir yaşam döngüsü erken aşaması sırasında sık erişilen, yalnızca gerekli veri kümesini göz önünde bulundurun bazen iki hafta sonra ve bir ay sonra ve sonrasında nadiren erişilir. Bu senaryoda, sık erişimli depolama erken aşamalarında en iyisidir, seyrek erişimli depolama, arada sırada erişim için en uygun ve Arşiv depolama en iyi katmanı veri yaş sonra bir aydan uzun bir seçenektir. Depolama katmanları verilerin yaşını açısından ayarlayarak, ihtiyaçlarınız için en uygun maliyetli depolama seçenekleri tasarlayabilirsiniz. Bu geçiş elde etmek için yaşam döngüsü yönetimi ilkeleri için harika katmanları eskime verileri taşımak kullanılabilir.

## <a name="storage-account-support"></a>Depolama hesabı desteği

Yaşam Döngüsü Yönetimi İlkesi ile hem genel amaçlı v2 (GPv2) hesabı ve Blob Depolama hesabı. Varolan genel amaçlı (GPv1) hesabı, Azure portalında basit bir tek tıklama işlemiyle aracılığıyla GPv2 hesabına dönüştürebilir. Daha fazla bilgi için bkz. [Azure depolama hesabı seçenekleri](../common/storage-account-options.md).  

## <a name="pricing"></a>Fiyatlandırma 

Yaşam döngüsü yönetimi ücretsiz önizleme olarak özelliğidir. Müşteriler normal işlem maliyetini ücretlendirilir [Blobları listeleme](https://docs.microsoft.com/rest/api/storageservices/list-blobs) ve [Blob katmanını ayarlama](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) API çağrıları. Bkz: [blok blobu fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/) fiyatlandırması hakkında daha fazla bilgi edinmek için.

## <a name="register-for-preview"></a>Önizleme için kaydolun 
Genel önizlemeye kaydolmak için aboneliğinize bu özelliği kaydetmek için bir istek göndermeniz gerekir. (Birkaç gün içinde) isteğiniz onaylandıktan sonra tüm mevcut ve yeni GPv2 veya Blob Depolama hesabında Batı ABD 2, Batı Orta ABD ve Batı Avrupa özelliğinin etkin olacaktır. Önizleme sırasında yalnızca blok blob desteklenir. Tarife ulaşana kadar ile çoğu önizleme olarak, bu özelliği üretim iş yükleri için kullanılmamalıdır

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
Özellik Onaylandı ve uygun şekilde kaydedildiğini, "Kaydedildi" durumu almanız gerekir.

### <a name="cli-20"></a>CLI 2.0

Bir istek göndermek için: 
```cli
az feature register --namespace Microsoft.Storage --name DLM
```
Aşağıdaki komutla kayıt onay durumunu kontrol edebilirsiniz:
```cli
az feature show --namespace Microsoft.Storage --name DLM
```
Özellik Onaylandı ve uygun şekilde kaydedildiğini, "Kaydedildi" durumu almanız gerekir. 


## <a name="add-or-remove-policies"></a>Ekleme veya kaldırma ilkeleri 

Ekleme, düzenleme veya Azure portalını kullanarak bir ilkeyi kaldırdığınızda [PowerShell](https://www.powershellgallery.com/packages/AzureRM.Storage/5.0.3-preview), [REST API'leri](https://docs.microsoft.com/rest/api/storagerp/storageaccounts/createorupdatemanagementpolicies), ya da istemci araçları aşağıdaki dillerde: [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/8.0.0-preview), [Python](https://pypi.org/project/azure-mgmt-storage/2.0.0rc3/), [Node.js]( https://www.npmjs.com/package/azure-arm-storage/v/5.0.0), [Ruby](   https://rubygems.org/gems/azure_mgmt_storage/versions/0.16.2). 

### <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesabınıza gitmek için Tüm Kaynaklar’ı ve ardından depolama hesabınızı seçin.

3. Ayarlar dikey penceresinde **yaşam döngüsü yönetimi** görüntülemek ve/veya ilkelerini değiştirmek için Blob hizmeti altında gruplandırılır.

### <a name="powershell"></a>PowerShell

```powershell
$rules = '{ ... }' 

Set-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName] -Policy $rules 

Get-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName]
```

> [!NOTE]
Depolama hesabınız için güvenlik duvarı kuralları etkinleştirirseniz, yaşam döngüsü yönetimi istekleri engellenebilir. Özel durumlar sağlayarak engelini kaldırabilirsiniz. Daha fazla bilgi için özel durumlar kısmına bakın [güvenlik duvarları ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

## <a name="policies"></a>İlkeler

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


Bir ilke içinde iki parametreler gereklidir:

| Parametre adı | Parametre türü | Notlar |
|----------------|----------------|-------|
| sürüm        | Bir dize olarak ifade edilen `x.x` | Önizleme sürüm numarasıdır 0,5 |
| rules          | Kural nesnelerinin bir dizisi | Her bir ilkede en az bir kural gerekir. Önizleme sırasında ilke başına en fazla 4 kuralları belirtebilirsiniz. |

Bir kural içinde Gerekli Parametreler şunlardır:

| Parametre adı | Parametre türü | Notlar |
|----------------|----------------|-------|
| Ad           | Dize | Kural adı, alfasayısal karakterlerin herhangi bir birleşimini içerebilir. Kural adı büyük/küçük harf duyarlıdır. Bir ilke içinde benzersiz olmalıdır. |
| type           | Bir sabit listesi değeri | Geçerli Önizleme değeri `Lifecycle` |
| tanım     | Yaşam döngüsü kuralı tanımlayan bir nesne | Her tanım, bir filtre kümesi ve bir eylem kümesi ile yapılır. |

## <a name="rules"></a>Kurallar

Her kural tanımı bir filtre kümesi ve bir eylem kümesi içerir. Aşağıdaki örnek kural önekiyle temel blok blobları için katman değiştirir `container1/foo`. İlkede, bu kuralları olarak tanımlanmıştır:

- Depolama 30 gün sonra son değiştirilme seyrek erişimli blob katmanı
- 90 gün sonra son değiştirilme BLOB arşiv depolama katmanı
- BLOB, son değiştirme sonrasında 2,555 gün (7 yıl) Sil
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

Filtreler, BLOB Depolama hesabında bir alt kural eylemi sınırlayın. Birden çok filtre tanımlanırsa, bir mantıksal `AND` tüm filtreleri gerçekleştirilir.

Önizleme süresince geçerli filtreler aşağıdakileri içerir:

| Filtre adı | Filtre türü | Notlar | Gereklidir |
|-------------|-------------|-------|-------------|
| blobTypes   | Önceden tanımlanmış bir sabit listesi değerleri dizisi. | Yalnızca önizleme sürümü için `blockBlob` desteklenir. | Evet |
| prefixMatch | Olması eşleşecek şekilde ön ekleri için dize dizisi. Bir önek dizesi, bir kapsayıcı adı ile başlamalıdır. Örneğin, tüm blobları altında "https://myaccount.blob.core.windows.net/mycontainer/mydir/..." eşleşmesi gereken bir kural için "mycontainer/Dizinim" önekidir. | Tanımlı değilse, bu kural hesabındaki tüm bloblar için geçerlidir. | Hayır |

### <a name="rule-actions"></a>Kural eylemi

Yürütme koşul karşılandığında eylemler için filtrelenmiş bloblara uygulanır.

Önizleme'de, katmanlama ve blob silme işlemi ve blob anlık görüntülerin silinmesi, yaşam döngüsü yönetimini destekler. Her kural, BLOB veya blob anlık görüntüleri üzerinde tanımlı en az bir eylem olmalıdır.

| Eylem        | Temel Blob                                   | Anlık Görüntü      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Sık erişimli katmanı şu anda bloblarını destekler         | Desteklenmiyor |
| tierToArchive | Sık erişimli veya seyrek erişimli katman şu anda bloblarını destekler | Desteklenmiyor |
| delete        | Desteklenen                                   | Desteklenen     |

>[!NOTE] 
Aynı bloba birden fazla eylem tanımlanmazsa, yaşam döngüsü yönetimi için blob ucuz eylemini uygular. (örneğin, eylem `delete` eyleminden daha ucuz `tierToArchive`. Eylem `tierToArchive` eyleminden daha ucuz `tierToCool`.)

Önizleme aşamasında olan eylem yürütme koşullar üzerinde yaş temel alır. Son değiştirme zamanı yaş izlemek ve yaş izlemek için anlık görüntüleri kullanan anlık görüntü oluşturma zamanı blob için temel blob kullanır.

| Eylem yürütme durumu | Koşul değeri | Açıklama |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Yaş gün cinsinden belirten bir tamsayı değeri | Temel blob eylemler için geçerli bir koşul |
| daysAfterCreationGreaterThan     | Yaş gün cinsinden belirten bir tamsayı değeri | Blob anlık görüntüsü eylemler için geçerli bir koşul | 

## <a name="examples"></a>Örnekler
Aşağıdaki örnekler, yaygın senaryolarda yaşam döngüsü ilkesi kuralları ile nasıl ekleyebileceğiniz gösterilmektedir.

### <a name="move-aging-data-to-a-cooler-tier"></a>Bir katmana eskime veri taşıma

Aşağıdaki örnek ön ekine sahip blok blobları geçiş yapmayı gösteren `container1/foo` veya `container2/bar`. İlke, 30 günden önce seyrek erişimli depolamaya içinde değiştirilmesini BLOB'ları ve Arşiv katmanına 90 gün içinde değiştirilmemiş blobları geçişleri:

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

Bazı veriler bulutta boşta kalır ve depolandıktan sonra, olursa, nadiren erişilir. Bu veriler, alınan hemen sonra arşivlenecek en iyisidir. Aşağıdaki yaşam döngüsü ilkesi alma, verileri arşivlemek için yapılandırılır. Bu örnekte geçişleri blok blobları depolama hesabında kapsayıcı içindeki `archivecontainer` hemen içine bir arşiv katmanı. Hemen geçiş bloblarda 0 gün sonra son değiştirilme zamanı işlevi tarafından gerçekleştirilir:

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

Bazı veriler, gün veya ay maliyetleri düşürmeyi veya kamu yönetmeliklerine uyum sağlamak için oluşturulduktan sonra süresi dolacak şekilde beklenmektedir. Yaşam döngüsü yönetim ilkesini verileri süresi dolacak şekilde veri yaş üzerinde temel silme işlemi tarafından ayarlanabilir. Aşağıdaki örnek, tüm blok BLOB'ları (ile belirtilen önek) silen bir ilke 365 günden eski gösterir.

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

## <a name="next-steps"></a>Sonraki adımlar

Yanlışlıkla silme işleminden sonra veri kurtarma işlemleri gerçekleştirmeyi öğreneceksiniz:

- [Azure depolama BLOB'ları için geçici silme ](../blobs/storage-blob-soft-delete.md)
