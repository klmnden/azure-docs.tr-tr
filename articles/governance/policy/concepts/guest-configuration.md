---
title: Bir sanal makine içeriğini denetim işlemini anlama
description: Konuk yapılandırma Azure İlkesi içinde bir Azure sanal makine ayarlarını denetlemek için nasıl kullandığını öğrenin.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/27/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: e6621172734ea02f971bd5064b403ad4844210a3
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960777"
---
# <a name="understand-azure-policys-guest-configuration"></a>Azure İlkesi'nin Konuk yapılandırma anlama

Ek denetim ve [düzeltme](../how-to/remediate-resources.md) Azure kaynakları, Azure İlkesi ayarları bir sanal makine içinde denetim. Doğrulama Konuk yapılandırma uzantısı ve istemci tarafından gerçekleştirilir. İstemcisi aracılığıyla uzantısı gibi işletim sistemi yapılandırması, uygulama yapılandırması veya varlığı, ortam ayarlarını ve diğer ayarlarını doğrular.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="extension-and-client"></a>Uzantı ve istemci

Bir sanal makine içinde ayarlarını denetlemek için bir [sanal makine uzantısı](../../../virtual-machines/extensions/overview.md) etkinleştirilir. Uzantı geçerli ilke ataması ve karşılık gelen yapılandırma tanımını indirir.

### <a name="register-guest-configuration-resource-provider"></a>Konuk yapılandırma kaynak sağlayıcısını kaydetme

Konuk yapılandırma kullanabilmeniz için kaynak sağlayıcısını kaydetmeniz gerekir. Portal veya PowerShell aracılığıyla kaydedebilirsiniz. Bir konuk yapılandırma ilkesi atamasını portal üzerinden yapılır, kaynak sağlayıcısı otomatik olarak kaydedilir.

#### <a name="registration---portal"></a>Kayıt - Portal

Azure portalı üzerinden Konuk yapılandırması için kaynak sağlayıcısını kaydetmek için aşağıdaki adımları izleyin:

1. Azure portalını başlatma ve tıklayarak **tüm hizmetleri**. Arayın ve seçin **abonelikleri**.

1. Bulun ve Konuk yapılandırma için etkinleştirmek istediğiniz aboneliğe tıklayın.

1. Soldaki menüde **abonelik** sayfasında **kaynak sağlayıcıları**.

1. Filtre uygulamak veya bulduktan kadar kaydırın **Microsoft.GuestConfiguration**, ardından **kaydetme** aynı satırda.

#### <a name="registration---powershell"></a>Kayıt - PowerShell

PowerShell üzerinden Konuk yapılandırması için kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell
Register-AzResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
```

### <a name="validation-tools"></a>Doğrulama Araçları

Sanal makinenin içinde denetim çalıştırmak için yerel Araçlar Konuk yapılandırma istemcisi kullanır.

Aşağıdaki tabloda, desteklenen her işletim sisteminde kullanılan yerel Araçlar listesi gösterilmektedir:

|İşletim sistemi|Doğrulama Aracı|Notlar|
|-|-|-|
|Windows|[Microsoft Desired State Configuration](/powershell/dsc) v2| |
|Linux|[Chef InSpec](https://www.chef.io/inspec/)| Ruby ve Python Konuk Configuration uzantısı tarafından yüklenir. |

### <a name="validation-frequency"></a>Doğrulama sıklığı

Konuk yapılandırma istemcisi için yeni içerik 5 dakikada denetler. Bir konuk ataması alındıktan sonra ayarları 15 dakikalık bir aralıkta denetlenir. Denetim tamamlandıktan hemen sonra sonuçlar Konuk yapılandırma kaynak sağlayıcısı için gönderilir. Bir ilke olduğunda [değerlendirme tetikleyici](../how-to/get-compliance-data.md#evaluation-triggers) gerçekleşir, makinenin durumu, Konuk yapılandırma kaynak sağlayıcısı için yazılır. Bu olay, Azure İlkesi, Azure Resource Manager özelliklerini değerlendirmek neden olur. İsteğe bağlı bir ilke değerlendirmesi Konuk yapılandırma kaynak Sağlayıcısı'ndan en son değeri alır. Ancak, bu yapılandırma sanal makine içinde yeni bir denetim tetiklemediğini.

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
> Konuk yapılandırma, desteklenen bir işletim sistemi çalıştıran tüm sunucuları denetleyebilirsiniz.  Özel bir görüntü kullanan sunucular denetlemek istiyorsanız, çoğaltmak gereken **Deployıfnotexists** tanımı ve değiştirme **varsa** görüntü özelliklerinizi eklemek için bölümü.

### <a name="unsupported-client-types"></a>Desteklenmeyen istemci türleri

Aşağıdaki tabloda, desteklenmeyen bir işletim sistemleri listelenmektedir:

|İşletim sistemi|Notlar|
|-|-|
|Windows istemcisi | İstemci işletim sistemleri (örneğin, Windows 7 ve Windows 10) desteklenmez.
|Windows Server 2016 Nano sunucu | Desteklenmiyor.|

### <a name="guest-configuration-extension-network-requirements"></a>Konuk yapılandırma uzantısı ağ gereksinimleri

Azure Konuk yapılandırma kaynak sağlayıcısı ile iletişim kurmak için sanal makinelerin giden bağlantı noktasında Azure veri merkezleri erişmesi **443**. Özel bir sanal ağ ile Azure kullanıyorsanız ve giden trafiğe izin verme, özel durumları kullanarak yapılandırılmalıdır [ağ güvenlik grubu](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) kuralları. Şu anda, Azure İlkesi Konuk yapılandırması için bir hizmet etiketi yok.

IP adresi listeleri için indirebileceğiniz [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Bu dosya haftalık olarak güncelleştirilir ve şu anda dağıtılmış aralıkları ve IP adreslerinde gelecekte yapılacak değişiklikleri vardır. Vm'lerinizin dağıtıldığı bölgeler IP'ler giden erişime izin vermek yeterlidir.

> [!NOTE]
> Azure veri merkezi IP adresi XML dosyası Microsoft Azure veri merkezlerinde kullanılan IP adresi aralıklarını listeler. İşlem, SQL ve depolama aralıkları dosyası içerir.
> Güncelleştirilen bir dosya haftalık olarak gönderilir. Dosya şu anda dağıtılmış aralıkları ve IP adreslerinde gelecekte yapılacak değişiklikleri yansıtır. Dosyada görünen yeni aralıklar en az bir hafta boyunca veri merkezlerinde kullanılmaz.
> Her hafta yeni XML dosyasını indirmek için iyi bir fikirdir. Ardından, Azure'da çalışan hizmetleri doğru şekilde tanımlamak üzere sitenizde güncelleştirin. Azure ExpressRoute kullanıcıların bu dosyayı her ayın ilk haftasında Azure alanındaki Border Gateway Protocol (BGP) reklamı güncelleştirmek için kullanıldığını unutmamalısınız.

## <a name="guest-configuration-definition-requirements"></a>Konuk yapılandırma tanımı gereksinimleri

İki ilke tanımları, Konuk yapılandırma Çalıştır her denetim gerektiren bir **Deployıfnotexists** tanımı ve bir **denetim** tanımı. **Deployıfnotexists** tanımını desteklemek için konuk yapılandırma aracısı ve diğer bileşenleri ile bir sanal makine hazırlama için kullanılan [Doğrulama Araçları](#validation-tools).

**Deployıfnotexists** ilke tanımı doğrular ve düzeltir aşağıdaki öğeleri:

- Doğrulama sanal makine değerlendirilecek bir yapılandırmayı atanmıştır. Hiçbir atama şu anda mevcutsa ataması Al ve sanal makine tarafından hazırlayın:
  - Sanal makineyi kullanarak kimlik doğrulaması bir [yönetilen kimlik](../../../active-directory/managed-identities-azure-resources/overview.md)
  - En son sürümünü yükleme **Microsoft.GuestConfiguration** uzantısı
  - Yükleme [Doğrulama Araçları](#validation-tools) ve gerekirse bağımlılıkları

Varsa **Deployıfnotexists** atamadır uyumlu olmayan, bir [düzeltme görev](../how-to/remediate-resources.md#create-a-remediation-task) kullanılabilir.

Bir kez **Deployıfnotexists** atamadır uyumlu, **denetim** ilke ataması yapılandırma atamasını uyumlu veya uyumlu olup olmadığını belirlemek için yerel doğrulama araçlarını kullanır.
Doğrulama Aracı sonuçları Konuk yapılandırma istemciye sağlar. İstemci, Konuk yapılandırma kaynak sağlayıcısı kullanılabilir hale getirir Konuk uzantısına sonuçları iletir.

Azure İlkesi kullanan Konuk yapılandırma kaynak sağlayıcıları **complianceStatus** rapor uyumluluk özelliğini **Uyumluluk** düğümü. Daha fazla bilgi için [uyumluluk verilerini alma](../how-to/getting-compliance-data.md).

> [!NOTE]
> Her Konuk yapılandırma tanımı için hem **Deployıfnotexists** ve **denetim** ilke tanımları bulunmalıdır.

Tüm yerleşik ilkeleri Konuk yapılandırması için girişim atamaları tanımlarında kullanın grubuna dahil edilmiştir. Yerleşik *[Önizleme]: Parola güvenlik ayarları içinde Linux ve Windows sanal makineleri denetle* girişim 18 ilkeleri içerir. Altı **Deployıfnotexists** ve **denetim** Windows ve Linux için üç çiftleri için ilke tanımı çiftleri.
Her, **Deployıfnotexists** [ilke tanımı kuralı](definition-structure.md#policy-rule) değerlendirilen sistemleri sınırlar.

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme örneklere [Azure ilkesi örnekleri](../samples/index.md)
- Gözden geçirme [İlkesi tanım yapısı](definition-structure.md)
- Gözden geçirme [ilke etkilerini anlama](effects.md)
- Anlamak için nasıl [programlı olarak ilkeler oluşturma](../how-to/programmatically-create.md)
- Bilgi edinmek için nasıl [uyumluluk verilerini al](../how-to/getting-compliance-data.md)
- Bilgi edinmek için nasıl [uyumlu olmayan kaynakları Düzelt](../how-to/remediate-resources.md)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/index.md) bölümünde yönetim gruplarını gözden geçirebilirsiniz
