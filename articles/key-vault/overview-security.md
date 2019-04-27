---
title: Azure Key Vault güvenlik | Microsoft Docs
description: Azure Key Vault, anahtarları ve gizli anahtarları için erişim izinlerini yönetin. Key Vault ve anahtar kasanızın güvenliğini sağlama için kimlik doğrulama ve yetkilendirme modeli anlatılmaktadır.
services: key-vault
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: barclayn
Customer intent: As a key vault administrator, I want to learn the options available to secure my vaults
ms.openlocfilehash: 43847b53fbf84fe42be3efdbd647767904a05fb8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60777670"
---
# <a name="azure-key-vault-security"></a>Azure Key Vault güvenliği

Azure Key Vault kullandığınız şekilde sertifikalar, bağlantı dizeleri ve bulutta parolalar gibi şifreleme anahtarlarını ve gizli korumak gerekir. Beri hassas depoladığını ve iş açısından kritik verilerin kasalarınızı ve bunlarda depolanan verilerin güvenliğini en üst düzeye çıkarmak için adımlar gerekir. Bu makalede bazı Azure anahtar kasası güvenlik tasarlarken dikkate almanız gereken kavramlar ele alınacaktır.

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

Bir Azure aboneliğinde bir anahtar kasası oluşturduğunuzda, aboneliğin Azure AD kiracısı ile otomatik olarak ilişkilendirilir. Herkes yönetin veya bir kasadan içeriği almaya çalışırken, Azure AD tarafından doğrulanmalıdır.

- Kimlik doğrulaması arayan kimliğini oluşturur.
- Yetkilendirme çağıran gerçekleştirebileceği işlemleri belirler. Yetkilendirme anahtar Kasası'nda bir birleşimini kullanan [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC) ve Azure anahtar kasası erişim ilkeleri.

### <a name="access-model-overview"></a>Erişim modeline genel bakış

Kasalarına erişmesi iki arabirim veya düzlemi üzerinden gerçekleşir. Bu düzlemleri yönetim düzlemi ve veri düzlemine ' dir.

- *Yönetim düzlemi* Key Vault kendisini yönetmek ve oluşturmak ve Kasaları silmek için kullanılan arabirimi olduğu yerdir. Ayrıca, anahtar kasası özelliklerini okumak ve erişim ilkelerini yönetme.
- *Veri düzlemi* anahtar kasasında depolanan verilerle çalışmanıza olanak sağlar. Ekleme, silme ve anahtarlara, parolalara ve sertifikalara değiştirmek.

Her iki düzlem bir anahtar kasasına erişmek için tüm çağıranların (kullanıcılar veya uygulamalar) kimlik doğrulaması yetkili ve gerekir. Her iki düzlem kimlik doğrulaması için Azure Active Directory (Azure AD) kullanın. Yetkilendirme için yönetim düzleminde rol tabanlı erişim denetimi (RBAC) ve veri düzlemine bir anahtar kasası erişim ilkesi kullanır.

Her iki düzlem için kimlik doğrulaması için tek bir mekanizma modelinin birçok faydası vardır:

- Kuruluşlar, kuruluş içindeki tüm anahtar kasalarına erişimi merkezi olarak kontrol edebilirsiniz.
- Bir kullanıcı ayrılırsa, bunlar anında erişim kuruluştaki tüm anahtar kasalarına kaybedersiniz.
- Kuruluş kimlik doğrulama seçenekleri Azure AD'de gibi ek güvenlik için multi-Factor authentication'ı etkinleştirmek için kullanarak özelleştirebilirsiniz

### <a name="managing-administrative-access-to-key-vault"></a>Key Vault yönetici erişimini yönetme

Bir kaynak grubunda bir anahtar kasası oluşturduğunuzda, Azure AD'yi kullanarak erişimi yönetin. Kullanıcılara vermek ya da bir kaynak grubundaki anahtar kasalarını yönetme olanağı gruplandırır. Belirli kapsam düzeyinde uygun RBAC rollerini atayarak erişim sağlayabilirsiniz. Bir kullanıcıya anahtar kasalarını yönetmesi için erişim vermek için önceden tanımlanmış bir atamanız `key vault Contributor` belirli bir kapsamda kullanıcı rolü. Bir RBAC rolü için aşağıdaki kapsamları düzeyleri atanabilir:

- **Abonelik**: Abonelik düzeyinde atanmış bir RBAC rolü, tüm kaynak grupları ve bu Abonelikteki kaynaklar için geçerlidir.
- **Kaynak grubu**: Kaynak grubu düzeyinde atanmış bir RBAC rolü, kaynak grubundaki tüm kaynaklar için geçerlidir.
- **Belirli kaynak**: Belirli bir kaynağa atanmış bir RBAC rolü bu kaynak için geçerli olur. Bu durumda, belirli bir anahtar kasası kaynaktır.

Önceden tanımlı birkaç rol vardır. Önceden tanımlanmış bir role gereksinimlerinize uymuyorsa, kendi rolünüzü tanımlayabilirsiniz. Daha fazla bilgi için [RBAC: Yerleşik roller](../role-based-access-control/built-in-roles.md).

> [!IMPORTANT]
> Bir kullanıcı varsa `Contributor` bir anahtar kasası yönetim düzleminde, kullanıcı izinler kendilerini veri düzlemine erişim anahtar kasası erişim ilkesini ayarlayarak. Kimlerin sıkı bir şekilde denetlemesi gerekir `Contributor` anahtar kasalarınıza erişimi rolü. Yalnızca yetkili kişiler erişebilir ve anahtar kasası, anahtarlara, parolalara ve sertifikalara yönetme emin olun.

<a id="data-plane-access-control"></a>
### <a name="controlling-access-to-key-vault-data"></a>Key Vault verilere erişimi denetleme

Key Vault erişim ilkeleri anahtarları, gizli diziler ve sertifika için ayrı ayrı izinler verir. Yalnızca anahtarları ve gizli diziler için bir kullanıcı erişimi verebilirsiniz. Anahtarlara, parolalara ve sertifikalara erişim izinlerini kasa düzeyinde yönetilir.

> [!IMPORTANT]
> Key Vault erişim ilkeleri, bir özel anahtar, gizli veya sertifika gibi ayrıntılı, nesne düzeyinde izinleri desteklemez. Bir kullanıcı oluşturma ve silme izni verildiğinde kullanıcılar ilgili anahtar kasasındaki tüm anahtarlar üzerinde bu işlemleri gerçekleştirebilir.

Bir anahtar kasası için erişim ilkeleri ayarlamak için kullanın [Azure portalında](https://portal.azure.com/), [Azure CLI](../cli-install-nodejs.md), [Azure PowerShell](/powershell/azureps-cmdlets-docs), veya [anahtar kasası yönetim REST API'leri](https://msdn.microsoft.com/library/azure/mt620024.aspx).

Kullanarak veri düzlemi erişimi kısıtlayabilirsiniz [sanal ağ hizmet uç noktaları Azure Key Vault için](key-vault-overview-vnet-service-endpoints.md). Yapılandırabileceğiniz [güvenlik duvarları ve sanal ağ kuralları](key-vault-network-security.md) ek bir güvenlik katmanı için.

## <a name="network-access"></a>Ağ erişimi

Onlara yönelik erişimi hangi IP adreslerine sahip belirterek kasalarınızı riskini azaltabilir. Azure Key Vault için sanal ağ hizmet uç noktalarını belirtilen bir sanal ağa erişimi sınırlamak sağlar. Bitiş IPv4 (internet Protokolü sürüm 4) adres aralıkları listesine erişimi sınırlamak sağlar. Key vault'ta dışındaki kaynaklarda bağlanan herhangi bir kullanıcı, erişim reddedildi.

Sonra güvenlik duvarı kuralları, izin verilen sanal ağları ya da IPv4 adres aralıklarını isteklerinde kaynaklanan, kullanıcılar yalnızca veri Key Vault'tan okuyabilir etkindir. Bu, aynı zamanda Key Vault Azure portalından erişmek için geçerlidir. Kullanıcılar, Azure portalından bir anahtar Kasası'na göz atabilir olsa da, kendi istemci makine izin verilenler listesinde değilse, bunlar liste anahtarlarını, gizli dizileri veya sertifikaları mümkün olmayabilir. Bu anahtar kasası Seçici tarafından diğer Azure hizmetleri de etkiler. Kullanıcılar anahtar kasalarının listesi bakın, ancak güvenlik duvarı kurallarını istemci makine engelliyorsa anahtarları listesi değil mümkün olabilir.

Azure Key Vault hakkında daha fazla bilgi için ağ adresi gözden geçirme [Azure anahtar kasası için sanal ağ hizmet uç noktaları](key-vault-overview-vnet-service-endpoints.md)

## <a name="monitoring"></a>İzleme

Anahtar kasası günlüğü kasanızdaki gerçekleştirilen etkinlikleri hakkındaki bilgileri kaydeder. Anahtar kasası günlükleri:

- Tüm başarısız istekler dahil olmak üzere REST API isteği kimlik doğrulaması
  - Anahtar işlemleri kendisini kasası. Bu işlemler, oluşturma, silme, erişim ilkeleri ayarlama ve etiketler gibi anahtar kasası özniteliklerini güncelleştirmeyi içerir.
  - Anahtarlar ve gizli anahtarları key vault'ta dahil olmak üzere:
    - Oluşturma, değiştirme veya bu anahtarları ve gizli anahtarları silme.
    - İmzalama, doğrulama, şifreleme, şifre çözme, sarmalama ve açma anahtarları, gizli ve listeleme anahtarlara ve parolalara (ve bunların sürümlerini) alma.
- Bir 401 yanıtına neden olan kimliği doğrulanmamış istekler. Hatalı biçimlendirilmiş ya da süresi dolmuş veya geçersiz bir belirtece sahip olan bir taşıyıcı belirtecine sahip olmayan isteklerinin verilebilir.

Bilgilerini günlüğe kaydetme, anahtar kasası işleminden sonra 10 dakika içinde erişilebilir. Depolama hesabınızdaki günlüklerinizi yönetmek size aittir. 

- Günlüklerinize erişebilecek kişileri kısıtlayarak güvenliklerini sağlamak için standart Azure erişim denetimi yöntemlerini kullanın.
- Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.

Güvenli Depolama yönetme öneri hesapları inceleyin [Azure depolama Güvenlik Kılavuzu](../storage/common/storage-security-guide.md)

## <a name="next-steps"></a>Sonraki Adımlar

- [Sanal ağ hizmet uç noktaları Azure Key Vault için](key-vault-overview-vnet-service-endpoints.md)
- [RBAC: Yerleşik roller](../role-based-access-control/built-in-roles.md)
- [Sanal ağ hizmet uç noktaları Azure Key Vault için](key-vault-overview-vnet-service-endpoints.md)
