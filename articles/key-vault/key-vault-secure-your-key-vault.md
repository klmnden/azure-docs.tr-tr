---
title: Azure anahtar kasanızın güvenliğini sağlama | Microsoft Docs
description: Azure Key Vault, anahtarları ve gizli anahtarları için erişim izinlerini yönetin. Key Vault ve anahtar kasanızın güvenliğini sağlama için kimlik doğrulama ve yetkilendirme modeli anlatılmaktadır.
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: ambapat
ms.openlocfilehash: 23f02f87b75cd41d1a56a388e4526be6d9a2e119
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52682750"
---
# <a name="secure-your-key-vault"></a>Anahtar kasanızın güvenliğini sağlama
Azure Key Vault, şifreleme anahtarlarını ve gizli anahtarları (örneğin, sertifikalar, bağlantı dizeleri ve parolalar gibi) koruyan bir bulut hizmetidir. Bu veriler hassas ve iş açısından kritik, anahtar kasalarınıza erişimi güvenli gerekir çünkü izin vererek yalnızca uygulamaların ve kullanıcıların yetkilendirdiniz. Bu makalede anahtar kasası erişim modeline genel bakış sağlar. Kimlik doğrulama ve yetkilendirme açıklanmakta ve erişim güvenliğinin nasıl sağlanacağını açıklar.

## <a name="overview"></a>Genel Bakış
İki ayrı arabirimler üzerinden bir anahtar kasası erişimi denetleme: *yönetim düzlemi* ve *veri düzlemi*. Her iki düzlem için uygun kimlik doğrulaması ve yetkilendirme, bir anahtar kasasına erişim, bir çağıranın (kullanıcı ya da uygulama) olması gerekir. Arayan kimliğini kimlik doğrulama oluşturur, yetkilendirme işlemleri belirler çağıran gerçekleştirebilirsiniz.

Kimlik doğrulaması için Azure Active Directory (Azure AD) her iki düzlem kullanın. Veri düzlemi, anahtar kasası erişim ilkesini kullanırken yetkilendirme için yönetim düzleminde rol tabanlı erişim denetimi (RBAC) kullanır.

## <a name="authenticate-by-using-azure-active-directory"></a>Azure Active Directory kullanarak kimlik doğrulaması
Bir Azure aboneliğinde bir anahtar kasası oluşturduğunuzda, aboneliğin Azure AD kiracısı ile otomatik olarak ilişkilendirilir. Tüm çağıranların bu kiracıya kaydedilmesi gerekir ve anahtar kasası erişim için kimlik doğrulaması gerekir. Bu gereksinim, her iki Yönetim düzlemi ve veri düzlemi erişimi için geçerlidir. Her iki durumda da, bir uygulamayı iki yolla Key Vault erişebilirsiniz:

* **Kullanıcı + uygulama erişimi**: oturum açmış kullanıcı adına anahtar kasasına erişen uygulamalar ile birlikte kullanılır. Azure PowerShell ve Azure portalı bu tür bir erişimin örnekleridir. Kullanıcılara erişim vermek için iki yolu vardır: 
  - Herhangi bir uygulamadan anahtar kasası erişim.
  - Access Key Vault yalnızca (Birleşik kimlik olarak adlandırılır) belirli bir uygulama kullandığınızda.

* **Salt uygulama erişim**: daemon Hizmetleri çalıştırmasına veya arka plan işleri uygulamaları ile kullanılır. Uygulamanın kimliğine anahtar kasası erişimi verilir.

Her iki uygulama türünde de, uygulamanın Azure AD'ye herhangi birini kullanarak doğrular [desteklenen kimlik doğrulama yöntemlerini](../active-directory/develop/authentication-scenarios.md)ve bir belirteç alır. Kullanılan kimlik doğrulama yöntemi, uygulama türüne bağlıdır. Daha sonra uygulama bu belirteci kullanır ve anahtar Kasası'na bir REST API isteği gönderir. Yönetim düzlemi istekleri, bir Azure Resource Manager uç noktasından yönlendirilir. Veri düzlemine erişirken, uygulamayı doğrudan bir anahtar kasası uç anlatıyor. Daha fazla bilgi için [kimlik doğrulama akışının tamamı](../active-directory/develop/v1-protocols-oauth-code.md). 

Kendisi için bir belirteç uygulamanın hangi gezmeyi bağlı uygulama istediği kaynak adı erişir. Kaynak adı, bir yönetim düzlemi uç noktası ya da Azure ortamına bağlı olarak, bir veri düzlemi uç değil. Daha fazla ayrıntı için bu makalenin devamındaki tabloya bakın.

Her iki düzlem için kimlik doğrulaması için tek bir mekanizmaya sahip olunması bazı avantajları vardır:

* Kuruluşlar, kuruluş içindeki tüm anahtar kasalarına erişimi merkezi olarak kontrol edebilirsiniz.
* Hemen bir kullanıcı ayrılırsa, kuruluştaki tüm anahtar kasalarına erişmesi isterse kaybeder.
* Kuruluşlar, Azure AD'de (örneğin, etkinleştirme çok faktörlü kimlik doğrulaması ek güvenlik için) kimlik doğrulama seçenekleri aracılığıyla özelleştirebilirsiniz.

## <a name="the-management-plane-and-the-data-plane"></a>Yönetim düzlemi ve veri düzlemi
Yönetim düzlemi, anahtar kasası kendisini yönetmek için kullanın. Bu öznitelikler yönetme ve veri düzlemi erişim ilkelerini ayarlama gibi işlemleri içerir. Veri düzlemi, eklemek, silmek, değiştirmek ve anahtarlara, parolalara ve Key Vault'ta depolanan sertifikaları kullanmak için kullanın.

Yönetim düzlemi ve veri düzlemi arabirimlerine farklı uç noktalar aşağıdaki tabloda listelenen aracılığıyla erişin. Tablodaki ikinci sütun, farklı Azure ortamlarında bu uç noktaların DNS adlarını açıklar. Üçüncü sütunda, her erişim düzleminde gerçekleştirebileceğiniz işlemler açıklanır. Her erişim düzlemi kendi erişim denetimi mekanizmasını de vardır. Yönetim düzlemi erişim denetimi, Azure Resource Manager rol tabanlı erişim denetimi (RBAC) kullanılarak ayarlanır. Veri düzlemi erişim denetimi, anahtar kasası erişim ilkesi kullanılarak ayarlanır.

| Erişim düzlemi | Erişim uç noktaları | İşlemler | Erişim denetimi mekanizması |
| --- | --- | --- | --- |
| Yönetim düzlemi |**Genel:**<br> management.azure.com:443<br><br> **Azure Çin 21Vianet:**<br> management.chinacloudapi.cn:443<br><br> **Azure ABD:**<br> management.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> management.microsoftazure.de:443 |Anahtar kasası oluşturma/okuma/güncelleştirme/silme <br> Anahtar kasası için erişim ilkeleri ayarlama<br>Anahtar kasası etiketlerini ayarlama |Azure Resource Manager RBAC |
| Veri düzlemi |**Genel:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Azure Çin 21Vianet:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure ABD:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |Anahtarlar için: şifresini çöz, şifrele, UnwrapKey, WrapKey, Doğrula, oturum, Get, List, güncelleştirme, oluştur, içeri aktarma, silme, yedekleme, geri yükleme<br><br> Parolalar için: Al, Listele, Ayarla, Sil |Anahtar kasası erişim ilkesi |

Yönetim düzlemi ve veri düzlemi erişim denetimleri birbirinden bağımsız olarak çalışır. Bir anahtar kasasındaki anahtarları kullanmak için bir uygulama erişim vermek istiyorsanız, örneğin, veri düzlemi erişimi vermek yeterlidir. Key Vault erişim ilkeleri aracılığıyla erişim. Buna karşılık, Key Vault özellikleri ve etiketler, ancak değil erişim verilerini (anahtarlar, parola veya sertifikalara) okumak için gereken bir kullanıcının yalnızca yönetim düzlemi erişimi gerekir. RBAC ile kullanıcı okuma erişimi atayarak erişim izni.

## <a name="management-plane-access-control"></a>Yönetim düzlemi erişim denetimi
Yönetim düzlemi, anahtar kasası kendisi gibi etkileyen işlemlerden oluşur:

- Oluşturma veya bir anahtar kasası siliniyor.
- Bir Abonelikteki kasaların listesini alınıyor.
- Key Vault özellikleri (örneğin, SKU ve etiketleri) alınıyor.
- Anahtarları ve gizli anahtarları için denetimi kullanıcı ve uygulama erişim Key Vault erişim ilkeleri ayarlama.

Yönetim düzlemi erişim denetimi, RBAC kullanır.  

### <a name="role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC)
Her Azure aboneliği bir Azure AD örneği sahiptir. Kullanıcılar, gruplar ve uygulamalar için Azure aboneliğinde Azure Resource Manager dağıtım modelini kullanan kaynakları yönetmek için bu dizindeki erişim. Bu tür bir erişim denetimi, RBAC adlandırılır. Bu erişimi yönetmek için [Azure portalı](https://portal.azure.com/), [Azure CLI araçları](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) veya [Azure Resource Manager REST API'lerini](https://msdn.microsoft.com/library/azure/dn906885.aspx) kullanabilirsiniz.

Bir kaynak grubunda anahtar kasası oluşturma ve Azure AD'yi kullanarak yönetim düzlemi erişim denetimi. Örneğin, kullanıcı veya grup kaynak grubundaki anahtar kasalarını yönetme olanağı verebilirsiniz.

Uygun RBAC rollerini atayarak erişim için kullanıcılara, gruplara ve uygulamalara belirli bir kapsamda verebilirsiniz. Örneğin, bir kullanıcıya anahtar kasalarını yönetmesi için erişim vermek için önceden tanımlanmış bir anahtar kasasına katkıda bulunan rolü belirli bir kapsamda bu kullanıcıya atayın. Kapsam, bir abonelik, kaynak grubu veya belirli bir anahtar kasası bu durumda olacaktır. Abonelik düzeyinde atanmış bir rol, tüm kaynak grupları ve bu Abonelikteki kaynaklar için geçerlidir. Kaynak grubu düzeyinde atanmış bir rol, kaynak grubundaki tüm kaynaklar için geçerlidir. Belirli bir kaynağa atanmış bir rol, ilgili kaynak için geçerlidir. Önceden tanımlı birkaç rol vardır (bkz [RBAC: yerleşik roller](../role-based-access-control/built-in-roles.md)). Önceden tanımlanmış bir role gereksinimlerinize uymuyorsa, kendi rolünüzü tanımlayabilirsiniz.

> [!IMPORTANT]
> Bir kullanıcı varsa, bir anahtar kasası yönetim düzleminde verebilir kendisini izinler katkıda bulunan veri düzlemi için anahtar kasası erişim ilkesini ayarlayarak erişim dikkat edin. Bu nedenle, anahtar kasalarınıza katkıda bulunan erişimine kimlerin sıkı bir şekilde denetlemesi gerekir. Yalnızca yetkili kişiler erişebilir ve anahtar kasası, anahtarlara, parolalara ve sertifikalara yönetme emin olun.
> 
> 

## <a name="data-plane-access-control"></a>Veri düzlemi erişim denetimi
Key Vault veri düzlemi işlemleri, anahtarlara, parolalara ve sertifikalara gibi depolanan nesnelere uygulanır. Anahtar işlemleri oluşturma, alma, güncelleştirme, liste, yedekleme ve geri yükleme anahtarları. Şifreleme işlemleri oturum dahil, doğrulayın, şifreleme, şifresini, sarmalama, sarmalamadan çıkarma, kümesi etiketleri ve diğer özniteliklerini ayarlayın. Benzer şekilde, gizli diziler üzerinde işlemler dahil get, set, liste silin.

Bir anahtar kasası için erişim ilkeleri ayarlanarak, veri düzlemi erişimi vermez. Bir kullanıcı, Grup veya uygulama için bu anahtar kasası erişim ilkeleri ayarlamak bir anahtar kasası yönetim düzleminde katkıda bulunan izinleri olmalıdır. Bir kullanıcı, Grup veya uygulama erişimini bir anahtar kasasındaki anahtarlar veya parolalar için belirli işlemleri gerçekleştirmek için izni verebilirsiniz. Key Vault için anahtar kasası erişim ilkesi girdileri en fazla 1024 destekler. Birkaç kullanıcıya veri düzlemi erişimi vermek için bir Azure AD güvenlik grubu oluşturun ve bu gruba kullanıcılar ekleyin.

### <a name="key-vault-access-policies"></a>Key Vault erişim ilkeleri
Key Vault erişim ilkeleri anahtarlara, parolalara ve sertifikalara ayrı ayrı izinler. Örneğin, yalnızca anahtarlar için kullanıcı erişim ve gizlilik için herhangi bir izin verebilirsiniz. Anahtar, parola veya sertifikalara erişim izni kasa düzeyinde verilir. Anahtar kasası erişim ilkesi, bir özel anahtar, gizli ya da sertifika gibi ayrıntılı, nesne düzeyinde izinleri desteklemez. Kullanabileceğiniz [Azure portalında](https://portal.azure.com/), [Azure CLI araçlarını](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs), veya [anahtar kasası yönetim REST API'leri](https://msdn.microsoft.com/library/azure/mt620024.aspx) erişim ilkeleri ayarlamak için bir anahtar kasası için.

> [!IMPORTANT]
> Anahtar kasası erişim ilkelerinin kasa düzeyinde uygulanır. Örneğin, bir kullanıcıya anahtar oluşturma ve silme izni verildiğinde kullanıcı bu işlemleri ilgili anahtar kasasındaki tüm anahtarlar üzerinde gerçekleştirebilir.

Erişim ilkelerini kullanmanın yanı sıra, ayrıca veri düzlemi erişimi kullanarak sınırlandırabilirsiniz [Azure anahtar kasası için sanal ağ hizmet uç noktaları](key-vault-overview-vnet-service-endpoints.md). Yapılandırdığınız [güvenlik duvarları ve sanal ağ kuralları](key-vault-network-security.md) ek bir güvenlik katmanı için.

## <a name="example"></a>Örnek
SSL, veri ve imzalama işlemleri için RSA 2048 bit anahtarı depolamak için Azure depolama için bir sertifika kullanan bir uygulama geliştirirken varsayalım. Bu uygulamayı çalıştıran Azure sanal makine'de (veya bir sanal makine ölçek kümesi) varsayalım. Tüm uygulama parolalarını depolamak için bir anahtar Kasası'nı kullanın ve Azure AD kimlik doğrulaması için uygulama tarafından kullanılan önyükleme sertifikasını depolayabilirsiniz.

Anahtarları ve depolanan gizli türlerinin bir özeti aşağıda verilmiştir:

* **SSL sertifikası**: SSL için kullanılır.
* **Depolama anahtarı**: depolama hesabına erişmek için kullanılır.
* **RSA 2048 bit anahtarı**: imzalama işlemleri için kullanılır.
* **Önyükleme sertifikası**: Azure AD kimlik doğrulaması için kullanılır. Erişimi verildikten sonra depolama anahtarını getirmek ve imzalama için RSA anahtarını kullanın.

Şimdi şimdi olanlar, yönetme, dağıtma ve bu uygulama denetimi kişilerle tanışalım. Bu örnekte üç rol kullanılacaktır.

* **Güvenlik ekibi**: office CSO (güvenlik Müdürü Yürüt) veya eşdeğer genellikle BT personelidir. Bu takım için gizli anahtarlarının doğru korunmasından sorumlu değildir. SSL sertifikaları ve RSA anahtarları, bağlantı dizeleri, imzalama için kullanılan depolama hesabı anahtarlarını örneklerindendir.
* **Geliştiriciler/operatörler**: kimin uygulamayı geliştirip Azure'da dağıtan istedim. Genellikle, olmadıklarını güvenlik ekibinin bir parçası ve bu nedenle SSL sertifikaları ve RSA anahtarları gibi hassas verilere erişimi olmamalıdır. Yalnızca dağıttıkları uygulama bu nesnelere erişimi olmalıdır.
* **Denetçiler**: geliştiriciler ve genel BT personelinden ayrı genellikle farklı bir dizi. Kullanım ve sertifikaları, anahtarları ve parolaları güvenlik standartlarıyla uyumluluğu sağlamak için bakım gözden geçirmek için kendi sorumluluğudur. 

Kapsamı bu uygulama, ancak ilgili dışında burada bahsedilecek olan rol daha mevcuttur. Aboneliği (veya kaynak grubu) bu rolü olan yönetici. Abonelik Yöneticisi güvenlik ekibine ilk erişim izinlerini ayarlar. Abonelik Yöneticisi, bu uygulama için gereken kaynakları içeren kaynak grubunu kullana güvenlik ekibine erişim verir.

Her bir rolün bu uygulama bağlamında gerçekleştirdiği işlemler aşağıda verilmiştir.

* **Güvenlik ekibi**
  * Anahtar kasası oluşturur.
  * Anahtar kasası günlüğü açar.
  * Anahtarlar/gizli ekler.
  * Olağanüstü durum kurtarma için anahtarların yedeğini oluşturur.
  * Anahtar kasası erişim ilkesini kullanıcılara ve uygulamalara belirli işlemleri gerçekleştirmek için izinleri vermek için ayarlar.
  * Anahtarları/parolaları düzenli aralıklarla yapar.
* **Geliştiriciler/operatörler**
  * Güvenlik ekibinden önyükleme ve SSL sertifikalarının (parmak izleri), depolama anahtarının (URI) ve imzalama anahtarının (anahtar URI'si) başvurularını alma.
  * Geliştirin ve anahtarları ve gizli anahtarları programlı olarak erişen uygulamayı dağıtın.
* **Denetçiler**
  * Uygun anahtar/parola kullanımını ve veri güvenliği standartlarına uyumu onaylamak üzere kullanım günlüklerini gözden geçirme.

Artık her rol ve uygulamanın atanmış görevleri gerçekleştirmesi için gerekli erişim izinlerinin ne görelim. 

| Kullanıcı rolü | Yönetim düzlemi izinleri | Veri düzlemi izinleri |
| --- | --- | --- |
| Güvenlik Ekibi |Key Vault Katkıda Bulunanı |Anahtarlar: yedekleme, oluşturma, silme, alma, içeri aktarma, listeleme, geri yükleme <br> Parolalar: tümü |
| Geliştiriciler/operatörler |Anahtar kasası iznine dağıttıkları sanal makinelerin, gizli dizileri anahtar kasasından getirebilmesi dağıtın. |None |
| Denetçiler |None |Anahtarlar: listeleme<br>Parolalar: listeleme |
| Uygulama |None |Anahtarlar: imzalama<br>Parolalar: imzalama |

> [!NOTE]
> Bunlar öznitelikleri için anahtar ve günlüklerde gönderilir olmayan gizli inceleyebilirsiniz için denetçiler izin anahtarları ve gizli anahtarları için olması gerekir. Bu öznitelikler, etiketler, etkinleştirme ve sona erme tarihleri içerir.
> 
> 

Key Vault izinlere ek olarak üç rolün tamamı da diğer kaynaklara erişim gerekir. Örneğin, Vm'leri (veya Azure App Service'in Web Apps özelliği) dağıtabilmek için geliştiriciler ve işleçleri de bu kaynak türlerine katkıda bulunan erişimi gerekir. Denetçiler, anahtar kasası günlüklerinin depolandığı depolama hesabına Okuma erişimine ihtiyacı vardır.

Bu makalenin odak noktası anahtar kasanıza erişimin güvenliğini sağlamak için biz yalnızca bu konuyla kavramları göstermektedir. Sertifikaları dağıtmak ve anahtarları ve gizli anahtarları program aracılığıyla erişme ile ilgili ayrıntıları başka bir yerde ele alınmaktadır. Örneğin:

- VM'ler için Key vault'ta depolanan sertifikaları dağıtma alınmıştır [müşteri tarafından yönetilen Key vault'tan dağıtma sertifikaların vm'lere](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) (blog gönderisi).
- [Azure anahtar kasası istemci örnekleri indirin](https://www.microsoft.com/download/details.aspx?id=45343) önyükleme sertifikası bir anahtar kasasına erişmek için Azure AD kimlik doğrulaması için nasıl kullanılacağı gösterilmektedir.

Azure portalını kullanarak çoğu erişim izinleri verebilirsiniz. Ayrıntılı izinler vermek için istenen sonucu elde etmek için Azure PowerShell veya CLI kullanın gerekebilir. 

Aşağıdaki PowerShell kod parçacıkları şu varsayımlara sahiptir:

* Azure AD Yöneticisi, üç rol (Contoso güvenlik ekibi, Contoso uygulama Devops ve Contoso uygulama denetçileri) temsil eden güvenlik grupları oluşturmuştur. Yönetici, ayrıca ait oldukları gruplara kullanıcılar eklemiştir.
* **ContosoAppRG** tüm kaynakların bulunduğu kaynak grubudur. **contosologstorage**, günlüklerin depolandığı yerdir. 
* Anahtar kasası **ContosoKeyVault**ve anahtar kasası günlükleri için kullanılan depolama hesabı **contosologstorage**, aynı Azure konumunda olmalıdır.

İlk Abonelik Yöneticisi atar `key vault Contributor` ve `User Access Administrator` güvenlik ekibine rolleri. Bu roller, güvenlik ekibinin diğer kaynaklara erişimi yönetmek izin ve ContosoAppRG kaynak grubundaki anahtar kasalarını yönetme.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Aşağıdaki komut dosyasını nasıl güvenlik ekibinin anahtar kasası oluşturabilir ve günlüğe kaydetme ve erişim izinleri ayarlama gösterir. Anahtar kasası erişim ilkesi izinleri hakkında daha fazla ayrıntı için bkz: [Azure Key Vault hakkında anahtarlara, parolalara ve sertifikalara](about-keys-secrets-and-certificates.md).

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -Name ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $devopsrole

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

Tanımlanan özel rol yalnızca aboneliğe atanabilir burada `ContosoAppRG` kaynak grubu oluşturulur. Diğer Aboneliklerdeki diğer projeler için aynı özel roller kullanılacaksa, kapsamı eklenen daha fazla abonelik olabilir.

Geliştiriciler ve işleçleri için "dağıtma/işlem" iznine yönelik özel rol ataması, kaynak grubu kapsamında yapılır. Bu kaynak grubunda oluşturulan VM'ler sağlar `ContosoAppRG` parolaları (SSL sertifikası ve önyükleme sertifikası) için erişim izni. Vm'leri, başka bir kaynak grubunda bir geliştiriciler tarafından oluşturulan ve gizli bir URI'leri olsa bile işleçleri ekip üyesi bu gizli dizileri erişemezsiniz.

Bu örnek, basit bir senaryo gösterir. Gerçek yaşam senaryoları daha karmaşık olabilir ve gereksinimlerinize göre anahtar kasanız için izinleri ayarlayabilirsiniz. Bu örnekte güvenlik ekibinin uygulamalarında başvurmaları gereken geliştiriciler ve işleçleri parola başvurularını (URI'ler ve parmak izleri) ve anahtar sağlar varsayıyoruz. Geliştiriciler ve işleçleri herhangi bir veri düzlemi erişimi gerekli değildir. Bu örnekte anahtar kasanızın güvenliğini sağlama üzerinde odaklanır. Güvenli hale getirmek için benzer göz önünde bulundurarak vermelisiniz [Vm'lerinizi](https://azure.microsoft.com/services/virtual-machines/security/), [depolama hesapları](../storage/common/storage-security-guide.md)ve diğer Azure kaynakları.

> [!NOTE]
> Bu örnek nasıl anahtar kasası erişim üretimde kilitlenir gösterir. Geliştiriciler kendi abonelik veya kaynak grubu, burada, kasalar, VM'ler ve depolama hesabı yönetmek için tam uygulama geliştirdikleri burada çalışırlarsa izinlerinin olması gerekir.

Anahtar kasanıza güvenli erişimi daha da önerilmektedir [Key Vault güvenlik duvarları ve sanal ağları yapılandırma](key-vault-network-security.md).

## <a name="resources"></a>Kaynaklar
* [Azure Active Directory rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md)
  
* [RBAC: Yerleşik Roller](../role-based-access-control/built-in-roles.md)
  
* [Resource Manager dağıtımını ve klasik dağıtımı anlama](../azure-resource-manager/resource-manager-deployment-model.md)
  
* [Azure PowerShell ile rol tabanlı erişim denetimini yönetme](../role-based-access-control/role-assignments-powershell.md)
  
* [Rol tabanlı access control REST API'si ile yönetme](../role-based-access-control/role-assignments-rest.md)
  
* [Ignite'tan Microsoft Azure için rol tabanlı erişim denetimi](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  Bu bir 2015 Microsoft video konferans anlatılmaktadır Ignite yönetim ve raporlama özellikleri erişin. Ayrıca, Azure AD'yi kullanarak Azure aboneliklerine erişimin güvenliğini sağlamak için en iyi uygulamaları açıklar.
* [OAuth 2.0 ve Azure AD kullanarak web uygulamalarına erişim yetkisi verme](../active-directory/develop/v1-protocols-oauth-code.md)
  
* [Anahtar kasası yönetim REST API'leri](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Bu belge, anahtar kasası erişim ilkesinin ayarlanması dahil olmak üzere anahtar kasanızı programlı olarak yönetmek REST API Başvurusu değil.
* [Anahtar kasası REST API'leri](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
* [Anahtar erişim denetimi](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
* [Gizli anahtar erişim denetimi](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
* [Ayarlama](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy) ve [Kaldır](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Remove-AzureRmKeyVaultAccessPolicy) PowerShell kullanarak anahtar kasası erişim ilkesi
  
## <a name="next-steps"></a>Sonraki adımlar
[Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma](key-vault-network-security.md)

Bir yöneticiye yönelik başlama öğreticisi için bkz. [Azure Anahtar Kasası ile çalışmaya başlama](key-vault-get-started.md).

Key Vault kullanım günlüğü hakkında daha fazla bilgi için bkz. [Azure Key Vault günlüğü](key-vault-logging.md).

Azure anahtar kasası ile anahtarları ve gizli anahtarları kullanma hakkında daha fazla bilgi için bkz. [anahtarlar ve gizlilikler hakkında](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Anahtar kasası ile ilgili sorularınız varsa bkz [forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

