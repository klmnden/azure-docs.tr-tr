---
title: Azure depolama yaşam döngüsünü yönetme
description: Yaşam döngüsü ilkesi kuralları geçiş eskime verileri seyrek erişimli ve Arşiv katmanları için sık erişimli'den oluşturmayı öğrenin.
services: storage
author: mhopkins-msft
ms.service: storage
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: mhopkins
ms.reviewer: yzheng
ms.subservice: common
ms.openlocfilehash: ce2559f62d29c7b062cfd1ad1dcb61146adfd91c
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66001751"
---
# <a name="manage-the-azure-blob-storage-lifecycle"></a>Azure Blob Depolama yaşam döngüsünü yönetme

Veri kümeleri benzersiz yaşam döngüleri vardır. Yaşam döngüsünde kişiler bazı veriler genellikle erişebilir. Ancak veriler eskidikçe önemli ölçüde erişim gereksinimini bırakır. Bazı veriler bulutta boşta kalır ve saklanmaya başlanan nadiren erişilir. Başka veri Setleri etkin bir şekilde okuyun ve kendi ömürleri değiştirilmiş olsa bazı veriler gün veya ay, oluşturulduktan sonra süresi dolar. Azure Blob Depolama yaşam döngüsü yönetimi, GPv2 ve Blob Depolama hesapları için zengin, kural tabanlı bir ilke sunar. Verilerinizi uygun erişim katmanları için geçiş ya da veri yaşam döngüsü sonunda sona ilkesini kullanın.

Yaşam döngüsü yönetim ilkesini sağlar:

- Geçiş bir depolama katmana (seyrek erişimli, arşivlemek için sık erişimli veya arşiv-seyrek erişimli sık erişimli) için BLOB'ları performansı ve maliyet açısından en iyi duruma getirme
- Döngülerini sonunda bloblarını silin
- Depolama hesabı düzeyinde günde bir kez çalıştırılacak kurallar tanımlama
- Kapsayıcıları ve blobları (ön ekleri filtre olarak kullanarak) bir alt kümesi için kuralları uygula

Burada veri alır sık erişim erken aşamalarında ancak yaşam döngüsü sırasında bir senaryo düşünün bazen iki hafta sonra. İlk aydan sonra veri kümesi nadiren erişilir. Bu senaryoda, sık erişimli depolama erken aşamalarında en iyisidir. Seyrek erişimli depolama, arada sırada erişim için en uygun seçenektir. Arşiv depolama en iyi katmanı veri yaş sonra bir ay boyunca seçenektir. Depolama katmanları verilerin yaşını açısından ayarlayarak, ihtiyaçlarınız için en uygun maliyetli depolama seçenekleri tasarlayabilirsiniz. Bu geçiş elde etmek için yaşam döngüsü yönetim ilkesi kurallarını eskime verileri harika katmanlara taşımak kullanılabilir.

## <a name="storage-account-support"></a>Depolama hesabı desteği

Yaşam döngüsü yönetim ilkesi hem genel amaçlı v2 ile kullanılabilir (GPv2) hesapları ve Blob Depolama hesapları. Azure portalında, varolan genel amaçlı (GPv1) hesabını GPv2 hesabına yükseltebilirsiniz. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).  

## <a name="pricing"></a>Fiyatlandırma

Yaşam döngüsü yönetimi özelliği ücretsizdir. Müşteriler normal işlem maliyetini ücretlendirilir [Blobları listeleme](https://docs.microsoft.com/rest/api/storageservices/list-blobs) ve [Blob katmanını ayarlama](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) API çağrıları. Silme işlemi ücretsizdir. Fiyatlandırma hakkında daha fazla bilgi için bkz. [blok blobu fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Yaşam döngüsü yönetimi özelliği tüm genel Azure bölgelerinde kullanılabilir.

## <a name="add-or-remove-a-policy"></a>Bir ilke ekleyip

Ekleme, düzenleme veya aşağıdaki yöntemlerden birini kullanarak bir ilke kaldırın:

* [Azure portal](https://portal.azure.com)
* [Azure PowerShell](https://github.com/Azure/azure-powershell/releases)
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)
* [REST API'ler](https://docs.microsoft.com/rest/api/storagerp/managementpolicies)

Bu makalede, portal ve PowerShell yöntemlerini kullanarak ilkesini yönetmek gösterilmektedir.  

> [!NOTE]
> Depolama hesabınız için güvenlik duvarı kuralları etkinleştirirseniz, yaşam döngüsü yönetimi istekleri engellenebilir. Bu istekler, özel durumlar sağlayarak engelini kaldırabilirsiniz. Gerekli atlama şunlardır: `Logging,  Metrics,  AzureServices`. Daha fazla bilgi için bkz: özel durumlar [güvenlik duvarları ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

### <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Seçin **tüm kaynakları** ve ardından depolama hesabınızı seçin.

3. Altında **Blob hizmeti**seçin **yaşam döngüsü yönetimi** görüntülemek veya ilkenizi değiştirmek için.

4. Aşağıdaki JSON içine yapıştırdığınız bir kural örneğidir **yaşam döngüsü yönetimi** portal sayfası.

   ```json
   {
     "rules": [
       {
         "name": "ruleFoo",
         "enabled": true,
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

5. Bu JSON örnek hakkında daha fazla bilgi için bkz: [ilke](#policy) ve [kuralları](#rules) bölümler.

### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell Betiği, bir ilke, depolama hesabınıza eklemek için kullanılabilir. `$rgname` Değişkeni, kaynak grubu adı ile başlatılması gerekir. `$accountName` Değişkeni, depolama hesabı adı ile başlatılması gerekir.

```powershell
#Install the latest module
Install-Module -Name Az -Repository PSGallery

#Initialize the following with your resource group and storage account names
$rgname = ""
$accountName = ""

#Create a new action object
$action = Add-AzStorageAccountManagementPolicyAction -BaseBlobAction Delete -daysAfterModificationGreaterThan 2555
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToArchive -daysAfterModificationGreaterThan 90
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToCool -daysAfterModificationGreaterThan 30
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -SnapshotAction Delete -daysAfterCreationGreaterThan 90

# Create a new filter object
# PowerShell automatically sets BlobType as “blockblob” because it is the only available option currently
$filter = New-AzStorageAccountManagementPolicyFilter -PrefixMatch ab,cd

#Create a new rule object
#PowerShell automatically sets Type as “Lifecycle” because it is the only available option currently
$rule1 = New-AzStorageAccountManagementPolicyRule -Name Test -Action $action -Filter $filter

#Set the policy
$policy = Set-AzStorageAccountManagementPolicy -ResourceGroupName $rgname -StorageAccountName $accountName -Rule $rule1
```

## <a name="azure-resource-manager-template-with-lifecycle-management-policy"></a>Yaşam Döngüsü Yönetimi İlkesi ile Azure Resource Manager şablonu

Yaşam döngüsü yönetimi, Azure Resource Manager şablonlarını kullanarak tanımlayabilirsiniz. RA-GRS GPv2 depolama hesabına bir yaşam döngüsü yönetimi ilkesi ile dağıtmak için bir örnek şablonu aşağıda verilmiştir.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "storageAccountName": "[uniqueString(resourceGroup().id)]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "location": "[resourceGroup().location]",
      "apiVersion": "2019-04-01",
      "sku": {
        "name": "Standard_RAGRS"
      },
      "kind": "StorageV2",
      "properties": {
        "networkAcls": {}
      }
    },
    {
      "name": "[concat(variables('storageAccountName'), '/default')]",
      "type": "Microsoft.Storage/storageAccounts/managementPolicies",
      "apiVersion": "2019-04-01",
      "dependsOn": [
        "[variables('storageAccountName')]"
      ],
      "properties": {
        "policy": {...}
      }
    }
  ],
  "outputs": {}
}
```

## <a name="policy"></a>İlke

Yaşam döngüsü yönetim ilkesi kuralları bir JSON belgesinde koleksiyonudur:

```json
{
  "rules": [
    {
      "name": "rule1",
      "enabled": true,
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

İlke kuralları koleksiyonudur:

| Parametre adı | Parametre türü | Notlar |
|----------------|----------------|-------|
| `rules`        | Kural nesnelerinin bir dizisi | İlke en az bir kural gerekir. İlke, en fazla 100 kuralları tanımlayabilirsiniz.|

Her bir kural ilke içinde çeşitli parametrelere sahiptir:

| Parametre adı | Parametre türü | Notlar | Gerekli |
|----------------|----------------|-------|----------|
| `name`         | String |Kural adı, alfasayısal en fazla 256 karakter içerebilir. Kural adı büyük/küçük harf duyarlıdır.  Bir ilke içinde benzersiz olmalıdır. | True |
| `enabled`      | Boolean | Bir kural geçici olarak izin vermek için isteğe bağlı bir boolean devre dışı. Bunu ayarlanmamışsa varsayılan değer True'dur. | False | 
| `type`         | Bir sabit listesi değeri | Geçerli geçerli tür `Lifecycle`. | True |
| `definition`   | Yaşam döngüsü kuralı tanımlayan bir nesne | Her tanım, bir filtre kümesi ve bir eylem kümesinden oluşur. | True |

## <a name="rules"></a>Kurallar

Her kural tanımı bir filtre kümesi ve bir eylem kümesi içerir. [Filtre kümesi](#rule-filters) adları nesneleri veya bir kapsayıcı içindeki nesneler belirli bir dizi kural eylemi sınırlar. [Eylem kümesi](#rule-actions) katmanı uygular veya filtrelenmiş nesne kümesini eylemleri sil.

### <a name="sample-rule"></a>Örnek kural

Aşağıdaki örnek kural içinde varolan nesnelere eylemleri çalıştırmak için hesap filtreleri `container1` ve başlayın `foo`.  

- 30 gün sonra son değiştirilme katmanını seyrek erişimli blob katmanı
- Katmanı son değiştirilme 90 gündür arşiv katmanında blob
- BLOB, son değiştirme sonrasında 2,555 gün (yedi yıl) Sil
- Anlık görüntü oluşturulduktan sonra 90 gün BLOB anlık görüntüleri silin

```json
{
  "rules": [
    {
      "name": "ruleFoo",
      "enabled": true,
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

Filtreler aşağıdakileri içerir:

| Filtre adı | Filtre türü | Notlar | Gerekli |
|-------------|-------------|-------|-------------|
| blobTypes   | Önceden tanımlanmış bir sabit listesi değerleri dizisi. | Geçerli yayın destekler `blockBlob`. | Evet |
| prefixMatch | Olması eşleşecek şekilde ön ekleri için dize dizisi. Her kural için 10 adede kadar ön ekleri tanımlayabilirsiniz. Bir önek dizesi, bir kapsayıcı adı ile başlamalıdır. Örneğin, tüm BLOB'ları altındaki eşleştirmek istiyorsanız `https://myaccount.blob.core.windows.net/container1/foo/...` prefixMatch bir kural için olan `container1/foo`. | PrefixMatch tanımlamazsanız, kural, depolama hesabındaki tüm bloblar için geçerlidir.  | Hayır |

### <a name="rule-actions"></a>Kural eylemi

Çalıştırma koşul karşılandığında eylem filtrelenmiş BLOB'ları uygulanır.

Yaşam döngüsü yönetimi, katmanlama ve silme BLOB ve blob anlık görüntüleri silme işlemi destekler. BLOB'ları veya blob anlık görüntüleri, her kural için en az bir eylem tanımlayın.

| Eylem        | Temel Blob                                   | Anlık görüntü      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Sık erişimli katmanı şu anda bloblarını destekler         | Desteklenmiyor |
| tierToArchive | Seyrek veya sık erişimli katmanı şu anda bloblarını destekler | Desteklenmiyor |
| sil        | Desteklenen                                   | Desteklenen     |

>[!NOTE]
>Aynı bloba birden fazla eylem tanımlarsanız, yaşam döngüsü yönetimi için blob ucuz eylemini uygular. Örneğin, eylem `delete` eyleminden daha ucuz `tierToArchive`. Eylem `tierToArchive` eyleminden daha ucuz `tierToCool`.

Çalıştırma koşulları üzerinde yaş temel alır. Temel blobları yaş izlemek için son değiştirilme zamanı kullanın ve yaş izlemek için anlık görüntüleri kullanmak anlık görüntü oluşturma zamanı blob.

| Koşul Çalıştır eylemi             | Koşul değeri                          | Açıklama                             |
|----------------------------------|------------------------------------------|-----------------------------------------|
| daysAfterModificationGreaterThan | Yaş gün cinsinden belirten bir tamsayı değeri | Koşul için temel blob eylemleri     |
| daysAfterCreationGreaterThan     | Yaş gün cinsinden belirten bir tamsayı değeri | Koşul için blob anlık görüntüsü eylemleri |

## <a name="examples"></a>Örnekler

Aşağıdaki örnekler, yaygın senaryolarda yaşam döngüsü ilkesi kuralları ile nasıl ekleyebileceğiniz gösterilmektedir.

### <a name="move-aging-data-to-a-cooler-tier"></a>Bir katmana eskime veri taşıma

Bu örnekte ön ekine sahip blok blobları geçiş gösterilmektedir `container1/foo` veya `container2/bar`. İlke, 30 günden önce seyrek erişimli depolamaya içinde değiştirilmesini BLOB'ları ve Arşiv katmanına 90 gün içinde değiştirilmemiş blobları geçişleri:

```json
{
  "rules": [
    {
      "name": "agingRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
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

Bazı veriler bulutta boşta kalır ve nadiren istenir; her zamankinden çok kez eriştiyseniz depolanır. Aşağıdaki yaşam döngüsü ilkesi içe alındığından sonra verileri arşivlemek için yapılandırılır. Bu örnekte geçişleri blok blobları depolama hesabında kapsayıcı içindeki `archivecontainer` içine bir arşiv katmanı. 0 gün sonra son değiştirilme zamanı bloblarda işlevi gören geçişi gerçekleştirilir:

```json
{
  "rules": [
    {
      "name": "archiveRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
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

Bazı veriler, gün veya ay oluşturulduktan sonra süresi dolacak şekilde beklenmektedir. Bir yaşam döngüsü yönetim ilkesi tarafından veri yaş üzerinde temel silme işlemi verileri süresi dolacak şekilde yapılandırabilirsiniz. Aşağıdaki örnek, tüm blok blobları 365 günden eski silen bir ilke gösterir.

```json
{
  "rules": [
    {
      "name": "expirationRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
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
  "rules": [
    {
      "name": "snapshotRule",
      "enabled": true,
      "type": "Lifecycle",
    "definition": {
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

## <a name="faq"></a>SSS

**Eylemler neden hemen çalışmaz, yeni bir ilke oluşturduğum?**  
Platform yaşam döngüsü ilkesi günde bir kez çalışır. Bir ilkeyi yapılandırdıktan sonra uygulamanın ilk kez çalıştırmak bazı eylemler 24 saate kadar sürebilir.  

**Ben el ile Arşivlenmiş bir blob rehydrated, nasıl bunu geri arşiv katmanı için geçici olarak taşınmasını engelleyebilirim?**  
Bir blobu başka bir erişim katmanı için bir erişim katmanı'ndan taşındığında, son değiştirme zamanı değişmez. Arşivlenen bir blobu sık erişimli katman için el ile yeniden doldurma, onu geri katmanı arşiv yaşam döngüsü yönetimi altyapısı tarafından taşınması. Bu blob geçici olarak etkileyen kuralı devre dışı bırakarak engelleyebilirsiniz. Sık erişimli katmanda kalıcı olarak kalması gerekiyorsa, blob başka bir konuma kopyalayabilirsiniz. Blob güvenli bir şekilde geri katmanı arşiv olarak taşınabilir, kural yeniden etkinleştirebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

Yanlışlıkla silme işleminden sonra veri kurtarma işlemleri gerçekleştirmeyi öğreneceksiniz:

- [Azure depolama BLOB'ları için geçici silme](../blobs/storage-blob-soft-delete.md)
