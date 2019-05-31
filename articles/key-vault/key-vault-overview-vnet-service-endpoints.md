---
title: Sanal ağ hizmet uç noktaları Azure Key Vault - Azure Key Vault için | Microsoft Docs
description: Sanal ağ hizmet uç noktaları için Key Vault genel bakış
services: key-vault
author: amitbapat
ms.author: ambapat
manager: barbkess
ms.date: 01/02/2019
ms.service: key-vault
ms.topic: conceptual
ms.openlocfilehash: 00274f8e15006f6f58a7c5f153bf0bbc0d26afb9
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66416431"
---
# <a name="virtual-network-service-endpoints-for-azure-key-vault"></a>Sanal ağ hizmet uç noktaları Azure Key Vault için

Azure Key Vault için sanal ağ hizmet uç noktalarını belirtilen bir sanal ağa erişimi sınırlamak sağlar. Bitiş IPv4 (internet Protokolü sürüm 4) adres aralıkları listesine erişimi sınırlamak sağlar. Key vault'ta dışındaki kaynaklarda bağlanan herhangi bir kullanıcı, erişim reddedildi.

Bu kısıtlama için önemli bir istisna yoktur. Bir kullanıcı kabul güvenilen Microsoft hizmetlerinin izin vermek için açma, bu hizmetlerinden gelen bağlantıları güvenlik duvarı üzerinden izin. Örneğin, bu hizmetler Office 365 Exchange Online içerir, Office 365 SharePoint Online, Azure işlem, Azure Resource Manager ve Azure Backup. Bu kullanıcılar hala geçerli bir Azure Active Directory belirteci sunmak gereken ve gerekir istenen işlemi gerçekleştirmek için sahip izinleri (erişim ilkeleri olarak yapılandırılır). Daha fazla bilgi için [sanal ağ hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="usage-scenarios"></a>Kullanım senaryoları

Yapılandırabileceğiniz [Key Vault güvenlik duvarları ve sanal ağlar](key-vault-network-security.md) varsayılan olarak (internet trafiğini dahil) tüm ağlardan erişime trafiği reddetmek için. Erişim uygulamalarınız için bir güvenli ağ sınırı oluşturmanıza izin vererek belirli bir Azure sanal ağları ve genel internet IP adresi trafiği için aralıklar, verebilirsiniz.

> [!NOTE]
> Key Vault güvenlik duvarları ve sanal ağ kuralları yalnızca geçerli [veri düzlemi](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) anahtar kasası. Anahtar kasası denetim düzlemi işlemleri (gibi oluşturma, silme ve işlemleri, erişim ilkeleri ayarlama, ayar güvenlik duvarları ve sanal ağ kuralları değiştirme), güvenlik duvarları ve sanal ağ kuralları tarafından etkilenmez.

Hizmet uç noktaları nasıl kullanabileceğinize ilişkin bazı örnekler şunlardır:

* Key Vault, şifreleme anahtarları, uygulama gizli dizilerini ve sertifikaları depolamak için kullandığınız ve genel internet'ten anahtar kasanız için erişimi engellemek istiyorsunuz.
* Anahtar kasanıza erişim yalnızca uygulamanız veya atanmış konaklar, kısa bir listesi için anahtar kasanıza bağlanabilmesi için kilitlemek istediğiniz.
* Azure sanal ağınızda çalışan bir uygulamaya sahiptir ve tüm gelen ve giden trafik için bu sanal ağ kilitlenmiştir. Uygulama parolaları veya sertifikaları getirme veya şifreleme anahtarlarını kullanmak için anahtar Kasası'na bağlanmak hala gerekir.

## <a name="configure-key-vault-firewalls-and-virtual-networks"></a>Key Vault güvenlik duvarları ve sanal ağları yapılandırma

Güvenlik duvarları ve sanal ağları yapılandırmak için gereken adımlar aşağıda verilmiştir. PowerShell, Azure CLI veya Azure portalı kullanıp kullanmadığınızı aşağıdaki adımları uygulayın.

1. Etkinleştirme [anahtar kasası günlüğü](key-vault-logging.md) ayrıntılı erişim günlükleri görmek için. Güvenlik duvarları ve sanal ağ kuralları bir anahtar kasasına erişim engellediğinizde bu Tanılama'da yardımcı olur. (Bu adım isteğe bağlıdır ancak uygulanması önemle önerilir.)
2. Etkinleştirme **hizmet uç noktalarını key vault için** hedef sanal ağlar ve alt ağlar için.
3. Güvenlik duvarları ve sanal özel ağlar, alt ağlar ve IPv4 adres aralıklarını, anahtar Kasası'na erişimi kısıtlamak bir anahtar kasası için sanal ağ kuralları ayarlayın.
4. Bu anahtar kasası, tüm güvenilen Microsoft Hizmetleri tarafından erişilebilir olması gereken, izin vermek için bu seçeneği etkinleştirin. **güvenilen Azure hizmetlerinin** anahtar Kasası'na bağlanmak için.

Daha fazla bilgi için [Azure anahtar Kasası'nı yapılandırma güvenlik duvarları ve sanal ağlar](key-vault-network-security.md).

> [!IMPORTANT]
> Güvenlik duvarı kuralları etkin olduktan sonra kullanıcılar yalnızca Key Vault gerçekleştirebilir [veri düzlemi](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) isteklerinde kaynaklanan olduğunda işlemlerine izin sanal ağlara veya IPv4 adres aralıklarını. Bu, aynı zamanda Key Vault Azure portalından erişmek için geçerlidir. Kullanıcılar, Azure portalından bir anahtar Kasası'na göz atabilir olsa da, kendi istemci makine izin verilenler listesinde değilse, bunlar liste anahtarlarını, gizli dizileri veya sertifikaları mümkün olmayabilir. Bu anahtar kasası Seçici tarafından diğer Azure hizmetleri de etkiler. Kullanıcılar anahtar kasalarının listesi bakın, ancak güvenlik duvarı kurallarını istemci makine engelliyorsa anahtarları listesi değil mümkün olabilir.


> [!NOTE]
> Aşağıdaki yapılandırma sınırlamaları unutmayın:
> * En fazla 127 sanal ağ kuralları ve 127 IPv4 kuralları izin verilir. 
> * Küçük adres aralıkları kullanan "/ 31" veya "/ 32" öneki boyutları desteklenmez. Bunun yerine, tek tek IP adresi kurallarını kullanarak bu aralığı yapılandırın.
> * IP ağ kuralları yalnızca genel IP adresleri için izin verilir. IP adresi aralıkları için özel ağlar (RFC 1918 ' tanımlandığı şekilde) ayrılmış IP kurallarında izin verilmez. Özel ağlar ile başlayan bir adres dahil **10.** , **172.16-31**, ve **192.168.** . 
> * Şu anda yalnızca IPv4 adresleri desteklenir.

## <a name="trusted-services"></a>Güvenilir hizmetler

İşte bir listesi, bir anahtar kasasına erişmek için izin verilen güvenilir hizmetlerin **izin güvenilir Hizmetleri** seçeneği etkin olduğunda.

|Güvenilir hizmet|Kullanım senaryoları|
| --- | --- |
|Azure sanal makineleri dağıtım hizmeti|[Sertifikalar, müşteri tarafından yönetilen Key Vault'tan Vm'lerine dağıtma](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/).|
|Azure Resource Manager şablon dağıtımı hizmeti|[Dağıtım sırasında güvenlik değerlerini geçirme](../azure-resource-manager/resource-manager-keyvault-parameter.md).|
|Azure Disk şifrelemesi birim şifreleme hizmeti|BitLocker anahtarı (Windows VM) veya DM parola (Linux VM) ve anahtar şifreleme anahtarı, sanal makine dağıtımı sırasında erişilebilsin. Böylece [Azure Disk şifrelemesi](../security/azure-security-disk-encryption.md).|
|Azure Backup|Yedekleme ve geri yükleme ilgili anahtar ve gizli dizi Azure sanal makineleri yedekleme sırasında kullanarak izin [Azure Backup](../backup/backup-introduction-to-azure-backup.md).|
|Exchange Online ve SharePoint Online|Müşteri anahtarı için Azure depolama hizmeti şifrelemesi ile erişmesine [müşteri anahtarı](https://support.office.com/article/Controlling-your-data-in-Office-365-using-Customer-Key-f2cd475a-e592-46cf-80a3-1bfb0fa17697).|
|Azure Information Protection|Kiracı anahtarı için erişim izni [Azure Information Protection.](https://docs.microsoft.com/azure/information-protection/what-is-information-protection)|
|Azure uygulama hizmeti|[Key Vault üzerinden Azure Web App sertifikası dağıtma](https://azure.github.io/AppService/2016/05/24/Deploying-Azure-Web-App-Certificate-through-Key-Vault.html).|
|Azure SQL Veritabanı|[Azure SQL veritabanı ve veri ambarı için kendi anahtarını Getir destekli saydam veri şifrelemesi](../sql-database/transparent-data-encryption-byok-azure-sql.md?view=sql-server-2017&viewFallbackFrom=azuresqldb-current).|
|Azure Storage|[Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption-customer-managed-keys.md).|
|Azure Data Lake Store|[Azure Data Lake Store içinde verilerin şifrelenmesi](../data-lake-store/data-lake-store-encryption.md) müşteri tarafından yönetilen bir anahtara sahip.|
|Azure databricks|[Hızlı, kolay ve işbirliğine dayalı Apache Spark tabanlı analiz hizmeti](../azure-databricks/what-is-azure-databricks.md)|



> [!NOTE]
> İlgili Key Vault erişim ilkeleri, anahtar kasasına erişim elde etmek karşılık gelen hizmetler izin verecek şekilde ayarlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtar kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md)
* [Azure Key Vault güvenlik duvarları ve sanal ağları yapılandırma](key-vault-network-security.md)
