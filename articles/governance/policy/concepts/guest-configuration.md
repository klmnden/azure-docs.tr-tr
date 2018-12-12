---
title: Azure İlkesi denetimleri içinde bir sanal makine performansını anlama
description: Konuk yapılandırma Azure İlkesi içinde bir Azure sanal makine ayarlarını denetlemek için nasıl kullandığını öğrenin.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/06/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 19bc8a58c1ad2115afdfd1d7e59b714ba19cadec
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53078897"
---
# <a name="understand-azure-policys-guest-configuration"></a>Azure İlkesi'nin Konuk yapılandırma anlama

Ek denetim ve [düzeltme](../how-to/remediate-resources.md) Azure kaynakları, Azure İlkesi ayarları bir sanal makine içinde denetimini özelliğine sahip. Doğrulama Konuk yapılandırma uzantısı ve istemci tarafından gerçekleştirilir. İstemcisi aracılığıyla uzantısı gibi işletim sistemi yapılandırması, uygulama yapılandırması veya varlığı, ortam ayarlarını ve diğer ayarlarını doğrular.

> [!IMPORTANT]
> Şu anda yalnızca **yerleşik** ilkeleri ile Konuk yapılandırma desteklenir.

## <a name="extension-and-client"></a>Uzantı ve istemci

Bir sanal makine içinde ayarlarını denetlemek için bir [sanal makine uzantısı](../../../virtual-machines/extensions/overview.md) etkinleştirilir. Uzantı geçerli ilke ataması ve karşılık gelen yapılandırma tanımını indirir.

### <a name="register-guest-configuration-resource-provider"></a>Konuk yapılandırma kaynak sağlayıcısını kaydetme

Konuk yapılandırma kullanabilmeniz için kaynak sağlayıcısını kaydetmeniz gerekir. Portal veya PowerShell aracılığıyla kaydedebilirsiniz.

#### <a name="registration---portal"></a>Kayıt - Portal

Azure portalı üzerinden Konuk yapılandırması için kaynak sağlayıcısını kaydetmek için aşağıdaki adımları izleyin:

1. Azure portalını başlatma ve tıklayarak **tüm hizmetleri**. Arayın ve seçin **abonelikleri**.

1. Bulun ve Konuk yapılandırma için etkinleştirmek istediğiniz aboneliğe tıklayın.

1. Soldaki menüde **abonelik** sayfasında **kaynak sağlayıcıları**.

1. Filtre uygulamak veya bulduktan kadar kaydırın **Microsoft.GuestConfiguration**, ardından **kaydetme** aynı satırda.

#### <a name="registration---powershell"></a>Kayıt - PowerShell

PowerShell üzerinden Konuk yapılandırması için kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
# Login first with Connect-AzureRmAccount if not using Cloud Shell
Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
```

### <a name="validation-tools"></a>Doğrulama Araçları

Sanal makinenin içinde denetim çalıştırmak için yerel Araçlar Konuk yapılandırma istemcisi kullanır.

Aşağıdaki tabloda, desteklenen her işletim sisteminde kullanılan yerel Araçlar listesi gösterilmektedir:

|İşletim sistemi|Doğrulama Aracı|Notlar|
|-|-|-|
|Windows|[Microsoft Desired State Configuration](/powershell/dsc) v2| |
|Linux|[Chef InSpec](https://www.chef.io/inspec/)| Ruby ve Python Konuk Configuration uzantısı tarafından yüklenir. |

### <a name="supported-client-types"></a>Desteklenen istemci türleri

Aşağıdaki tabloda, desteklenen işletim sistemi listesini Azure görüntülerinde gösterilmektedir:

|Yayımcı|Ad|Sürümler|
|-|-|-|
|Canonical|Ubuntu Server|14.04, 16.04 18.04|
|credativ|Debian|8, 9|
|Microsoft|Windows Server|2012 Datacenter, 2012 R2 Datacenter, 2016 Datacenter|
|OpenLogic|CentOS|7.3, 7.4 7.5|
|Red Hat|Red Hat Enterprise Linux|7.4, 7.5|
|SuSE|SLES|12 SP3|

> [!IMPORTANT]
> Konuk yapılandırma özel sanal makine görüntülerinde şu anda desteklenmiyor.

### <a name="unsupported-client-types"></a>Desteklenmeyen istemci türleri

Aşağıdaki tabloda, desteklenmeyen bir işletim sistemleri listelenmektedir:

|İşletim sistemi|Notlar|
|-|-|
|Windows istemcisi | İstemci işletim sistemleri (örneğin, Windows 7 ve Windows 10) desteklenmez.
|Windows Server 2016 Nano sunucu | Desteklenmiyor.|

## <a name="guest-configuration-definition-requirements"></a>Konuk yapılandırma tanımı gereksinimleri

İki ilke tanımları, Konuk yapılandırma Çalıştır her denetim gerektiren bir **Deployıfnotexists** ve **AuditIfNotExists**. **Deployıfnotexists** sanal makine yapılandırma Konuk aracısı ve diğer bileşenleri ile destekleyecek şekilde hazırlamak için kullanılan [Doğrulama Araçları](#validation-tools).

**Deployıfnotexists** ilke tanımı doğrular ve düzeltir aşağıdaki öğeleri:

- Doğrulama sanal makine değerlendirilecek bir yapılandırmayı atanmıştır. Hiçbir atama şu anda mevcutsa ataması Al ve sanal makine tarafından hazırlayın:
  - Sanal makineyi kullanarak kimlik doğrulaması bir [yönetilen kimlik](../../../active-directory/managed-identities-azure-resources/overview.md)
  - En son sürümünü yükleme **Microsoft.GuestConfiguration** uzantısı
  - Yükleme [Doğrulama Araçları](#validation-tools) ve gerekirse bağımlılıkları

Bir kez **Deployıfnotexists** , uyumlu olan **AuditIfNotExists** ilke tanımı, atanan yapılandırma atamasını uyumlu olup olmadığını belirlemek için yerel doğrulama araçlarını kullanır veya Uyumlu olmayan. Doğrulama Aracı sonuçları Konuk yapılandırma istemciye sağlar. İstemci, Konuk yapılandırma kaynak sağlayıcısı kullanılabilir hale getirir Konuk uzantısına sonuçları iletir.

Azure İlkesi kullanan Konuk yapılandırma kaynak sağlayıcıları **complianceStatus** rapor uyumluluk özelliğini **Uyumluluk** düğümü. Daha fazla bilgi için [uyumluluk verilerini alma](../how-to/getting-compliance-data.md).

> [!NOTE]
> Her Konuk yapılandırma tanımı için hem **Deployıfnotexists** ve **AuditIfNotExists** ilke tanımları bulunmalıdır.

Tüm yerleşik ilkeleri Konuk yapılandırması için girişim atamaları tanımlarında kullanın grubuna dahil edilmiştir. Adlı yerleşik girişim *[Önizleme]: denetim parola güvenlik ayarları Linux ve Windows sanal makineleri içinde* 18 ilkelerini içerir. Altı **Deployıfnotexists** ve **AuditIfNotExists** Windows ve Linux için üç çift çifti. Her durumda, yalnızca hedef mantıksal tanımındaki doğrular işletim sistemine göre değerlendirilir [ilke kuralı](definition-structure.md#policy-rule) tanımı.

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme örneklere [Azure ilkesi örnekleri](../samples/index.md)
- Gözden geçirme [İlkesi tanım yapısı](definition-structure.md)
- Gözden geçirme [ilke etkilerini anlama](effects.md)
- Anlamak için nasıl [programlı olarak ilkeler oluşturma](../how-to/programmatically-create.md)
- Bilgi edinmek için nasıl [uyumluluk verilerini al](../how-to/getting-compliance-data.md)
- Bilgi edinmek için nasıl [uyumlu olmayan kaynakları Düzelt](../how-to/remediate-resources.md)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/index.md) bölümünde yönetim gruplarını gözden geçirebilirsiniz