---
ms.assetid: ''
title: Sanal ağ hizmet uç noktaları için Azure anahtar kasası | Microsoft Docs
description: Sanal ağ hizmet uç noktaları için anahtar kasasına genel bakış
services: key-vault
author: amitbapat
ms.author: ambapat
manager: mbaldwin
ms.date: 08/31/2018
ms.service: key-vault
ms.workload: identity
ms.topic: conceptual
ms.openlocfilehash: c2696d5eb22443b565c48ef4f96d6e4a25827606
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44295013"
---
# <a name="virtual-network-service-endpoints-for-azure-key-vault"></a>Sanal ağ hizmet uç noktaları için Azure anahtar kasası

Key Vault için sanal ağ hizmet uç noktaları, belirtilen sanal ağ ve/veya IPv4 (Internet Protokolü sürüm 4) adres aralıkları listesi için erişimi kısıtlamak izin verin. Key vault'ta dışındaki kaynaklarda bağlanan herhangi bir çağırıcı erişimi reddedilir. Müşteri kabul-Office 365 Exchange Online, Office 365 SharePoint Online, Azure, Azure Resource Manager, Azure yedekleme vb. işlem gibi "Güvenilen Microsoft Hizmetleri" izin vermek için giriş bu hizmetlerinden gelen bağlantıları güvenlik duvarı üzerinden sağlayacaktır. Elbette bu tür çağıranlar geçerli bir AAD belirteç sunmasını yine ve istenen işlemi gerçekleştirmek için izinleri (erişim ilkeleri olarak yapılandırılır) olması gerekir. Daha fazla teknik ayrıntılar hakkında bilgi edinin [sanal ağ hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="usage-scenarios"></a>Kullanım senaryoları

Yapılandırabileceğiniz [Key Vault güvenlik duvarları ve sanal ağlar](key-vault-network-security.md) varsayılan olarak (Internet trafiğini dahil) tüm ağlardan erişime trafiği reddetmek için. Erişim belirli bir Azure sanal ağları ve/veya genel internet IP adresi trafiği için aralıklar, uygulamalarınız için bir güvenli ağ sınırı oluşturmanıza izin verilebilir.

> [!NOTE]
> Key Vault güvenlik duvarları ve sanal ağ kuralları yalnızca anahtar Kasası'na uygulamak [veri düzlemi](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control). Anahtar kasası denetim düzlemi işlemleri (gibi anahtar kasası oluşturma, silme, erişim ilkeleri, güvenlik duvarları ve sanal ağ kuralları ayarlama ayarlama işlemleri, Değiştir) güvenlik duvarları ve sanal ağ kuralları tarafından etkilenmez.

Örneğin,
* Key Vault, şifreleme anahtarları, uygulama parolaları, sertifikaları depolamak için kullandığınız ve genel internet'ten anahtar kasanız için erişimi engellemek istiyorsanız
* Anahtar kasanıza erişim yalnızca uygulamanızın veya atanmış konaklar kısa bir listesi için anahtar kasanıza bağlanabilmesi için kilitlemek istediğiniz
* Azure, sanal ağ (VNET) içinde çalışan bir uygulamaya sahip ve bu sanal ağ için tüm gelen ve giden trafiği kilitlenmiştir. Gizli dizileri veya sertifikaları getirilemedi veya şifreleme anahtarlarını kullanmak için anahtar Kasası'na bağlanmak uygulamanızı yine de gerekir.

## <a name="configure-key-vault-firewalls-and-virtual-networks"></a>Key Vault güvenlik duvarları ve sanal ağları yapılandırma

Güvenlik duvarları ve sanal ağları yapılandırmak için gereken adımlar aşağıda verilmiştir. Bu adımları hangi arabirimi (PowerShell, CLI, Azure portalı) güvenlik duvarı ve sanal ağ kuralları'kurmak için kullanacağınız bakılmaksızın aynı kalır.
1. İsteğe bağlıdır ancak uygulanması önemle önerilir: etkinleştirme [anahtar kasası günlüğü](key-vault-logging.md) ayrıntılı erişim günlükleri görmek için. Güvenlik duvarları ve sanal ağ kuralları bir anahtar kasasına erişim engelle. Bu, Tanılama'da yardımcı olur.
2. 'Hizmet uç noktaları için anahtar Kasası' hedef sanal ağlar ve alt ağı, başarılı etkinleştir
3. Güvenlik duvarları ve sanal özel ağlar, alt ağı, başarılı ve IPv4 adres aralıklarını, anahtar Kasası'na erişimi kısıtlamak bir anahtar kasası için sanal ağ kuralları ayarlayın.
4. Bu anahtar kasasına herhangi bir güvenilen Microsoft Hizmetleri tarafından erişilebilir olması gerekiyorsa ' Güvenilir Azure anahtar Kasası'na bağlanmak üzere hizmetlerini' izin verme seçeneğini etkinleştirmeniz gerekir.

Başvurmak [Azure anahtar kasası yapılandırma güvenlik duvarları ve sanal ağları](key-vault-network-security.md) ayrıntılı adım adım yönergeler için.

> [!IMPORTANT]
> Güvenlik duvarı kurallarını tüm anahtar kasası etkin olduğunda [veri düzlemi](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) işlemleri, yalnızca izin verilen sanal ağlara ya da IPv4 adres aralıklarını çağıranın isteği oluşturulduğunda gerçekleştirilebilir. Bu ayrıca, Azure Portalı'ndan anahtar kasasına erişmek için geçerlidir. Bir kullanıcının tarayıcı bir anahtar kasasına Azure portalından, ancak kendi istemci makine izin verilenler listesinde değilse, bunlar liste anahtarlar/gizli dizileri/sertifikaları mümkün olmayabilir. Bu ayrıca diğer Azure Hizmetleri tarafından 'Anahtar kasası Seçici' etkiler. Kullanıcılar anahtar kasalarının listesi bakın, ancak güvenlik duvarı kurallarını istemci makine engelliyorsa anahtarları listesinde değil.


> [!NOTE]
> * En fazla 127 bir sanal ağ kuralları ve 127 IPv4 kuralları izin verilir. 
> * Küçük adres aralıkları kullanarak "/ 31" veya "/ 32" öneki boyutları desteklenmez. Bu aralıklar, tek tek IP adresi kurallarını kullanarak yapılandırılmalıdır.
> * IP ağ kuralları yalnızca genel IP adresleri için izin verilir. IP adresi aralıkları için özel ağlar (RFC 1918 ' tanımlandığı şekilde) ayrılmış IP kurallarında izin verilmez. Özel ağlar ile başlayan bir adres içerir * 10.* * * 172.16. **, ve * 192.168. **. 
> * Şu anda yalnızca IPv4 adresleri desteklenir.

## <a name="trusted-services"></a>Güvenilir hizmetler
'İzin Hizmetleri güvenilir' seçeneği etkin olduğunda, bir anahtar kasasına erişmek için izin verilen güvenilir hizmetlerin listesi aşağıda verilmiştir.

|Güvenilir hizmet|Kullanım senaryoları|
| --- | --- |
|Azure Sanal Makineleri dağıtım hizmeti|[Müşteri tarafından yönetilen Key Vault'tan VM'ler için sertifikaları dağıtma](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)|
|Azure Resource Manager şablon dağıtımı hizmeti|[Dağıtım sırasında güvenlik değerlerini geçirme](../azure-resource-manager/resource-manager-keyvault-parameter.md)|
|Azure Disk Şifrelemesi birim şifreleme hizmeti|Etkinleştirmek için sanal makine dağıtımı sırasında BitLocker anahtarı (Windows VM) veya DM parola (Linux VM) ve anahtar şifreleme anahtarı erişmesine [Azure Disk şifrelemesi](../security/azure-security-disk-encryption.md)|
|Azure Backup|Yedekleme ve geri yükleme ilgili anahtarları ve gizli dizileri Azure VM yedeklemesi sırasında izin kullanarak [Azure yedekleme](../backup/backup-introduction-to-azure-backup.md)|
|Exchange Online ve SharePoint Online|Hizmet şifrelemesi ile müşteri anahtarı erişim izni [müşteri anahtarı](https://support.office.com/en-us/article/Controlling-your-data-in-Office-365-using-Customer-Key-f2cd475a-e592-46cf-80a3-1bfb0fa17697).|
|Azure Information Protection|Kiracı anahtarı için erişim izni [Azure Information Protection.](https://docs.microsoft.com/azure/information-protection/what-is-information-protection)|
|Uygulama Hizmetleri|[Key Vault üzerinden Azure Web App sertifikası dağıtma](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)|
|Azure SQL|[Azure SQL veritabanı ve veri ambarı için kendi anahtarını Getir destekli saydam veri şifrelemesi](../sql-database/transparent-data-encryption-byok-azure-sql.md?view=sql-server-2017&viewFallbackFrom=azuresqldb-current)|
|Azure Storage|[Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption-customer-managed-keys.md)|
|Azure Data Lake Store|[Azure Data Lake Store içinde verilerin şifrelenmesi](../data-lake-store/data-lake-store-encryption.md) anahtarı müşteri ile yönetilen|



> [!NOTE]
> İlgili key vault erişim ilkeleri, anahtar kasasına erişim elde etmek karşılık gelen hizmetler izin verecek şekilde ayarlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtar kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md)
* [Azure anahtar kasası güvenlik duvarları ve sanal ağları yapılandırma](key-vault-network-security.md)