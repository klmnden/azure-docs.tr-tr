---
title: Key vault - Azure anahtar kasası erişiminin güvenliğini sağlama | Microsoft Docs
description: Azure Key Vault, anahtarları ve gizli anahtarları için erişim izinlerini yönetin. Key Vault ve anahtar kasanızın güvenliğini sağlama için kimlik doğrulama ve yetkilendirme modeli anlatılmaktadır.
services: key-vault
author: amitbapat
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: 67925f2123f2a4f2524002eb075754c38fad4b42
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67118994"
---
# <a name="secure-access-to-a-key-vault"></a>Bir anahtar kasasına erişimin güvenliğini sağlama

Azure Key Vault, sertifikalar, bağlantı dizeleri ve parolalar gibi şifreleme anahtarlarını ve gizli koruyan bir bulut hizmetidir. Bu veri büyük/küçük harfe duyarlıdır ve iş açısından kritik, güvenli anahtar kasalarınıza erişimi yalnızca yetkili uygulamaların ve kullanıcıların vererek gerekir çünkü. Bu makalede anahtar kasası erişim modeline genel bakış sağlar. Kimlik doğrulama ve yetkilendirme açıklanmakta ve anahtar kasalarınıza erişim güvenliğinin nasıl sağlanacağını açıklar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="access-model-overview"></a>Erişim modeline genel bakış

Bir anahtar kasasına erişim, iki arabirim ile denetlenir: **yönetim düzlemi** ve **veri düzlemi**. Yönetim düzlemi, anahtar kasası kendi yönettiğiniz ' dir. Bu düzlemi işlemlerine dahil etmek oluşturma ve anahtar kasalarını silme, anahtar kasası özellikleri alınıyor ve erişim ilkeleri güncelleştiriliyor. Veri düzlemi, anahtar kasasında depolanan verilerle çalıştığınız ' dir. Ekleme, silme ve anahtarlara, parolalara ve sertifikalara değiştirmek.

Tüm çağıranların (kullanıcılar veya uygulamalar) ya da masasında bir anahtar kasasına erişmek için uygun kimlik doğrulaması ve yetkilendirme olması gerekir. Kimlik doğrulaması arayan kimliğini oluşturur. Yetkilendirme, çağırana yürütebilir hangi işlemleri belirler. 

Her iki düzlem kimlik doğrulaması için Azure Active Directory (Azure AD) kullanın. Yetkilendirme için yönetim düzleminde rol tabanlı erişim denetimi (RBAC) ve veri düzlemine bir anahtar kasası erişim ilkesi kullanır.

## <a name="active-directory-authentication"></a>Active Directory kimlik doğrulaması

Bir Azure aboneliğinde bir anahtar kasası oluşturduğunuzda, aboneliğin Azure AD kiracısı ile otomatik olarak ilişkilendirilir. Tüm çağıranlar, her iki düzlem bu kiracıda kaydetmek ve anahtar kasası erişim için kimlik doğrulaması gerekir. Her iki durumda da, uygulamaları, Key Vault iki yolla erişebilir:

- **Kullanıcı ve uygulama erişim**: Uygulamanın, oturum açmış kullanıcı adına anahtar kasası erişir. Azure PowerShell ve Azure portalı bu tür bir erişimin örnekleridir. İki yolla kullanıcı erişimi verilir. Kullanıcılar herhangi bir uygulamadan anahtar kasası erişim sağlayabilir veya belirli bir uygulama kullanmanız gerekir (olarak adlandırılan _bileşik kimlik_).
- **Salt uygulama erişim**: Uygulama bir arka plan programı hizmeti veya arka plan işi olarak çalıştırır. Uygulama kimliği, anahtar kasası erişimi verilir.

Her iki tür erişim için Azure AD ile uygulama kimliğini doğrular. Herhangi bir uygulama kullanan [desteklenen kimlik doğrulama yöntemi](../active-directory/develop/authentication-scenarios.md) uygulama türüne göre. Uygulama, bir kaynak masasında erişim vermek için bir belirteç alır. Bir Azure ortamına bağlı Yönetim veya veri düzlemi uç noktasında bir kaynaktır. Uygulama belirteci kullanır ve anahtar Kasası'na bir REST API isteği gönderir. Daha fazla bilgi edinmek için [kimlik doğrulama akışının tamamı](../active-directory/develop/v1-protocols-oauth-code.md).

Her iki düzlem için kimlik doğrulaması için tek bir mekanizma modelinin birçok faydası vardır:

- Kuruluşlar, kuruluş içindeki tüm anahtar kasalarına erişimi merkezi olarak kontrol edebilirsiniz.
- Bir kullanıcı ayrılırsa, bunlar anında erişim kuruluştaki tüm anahtar kasalarına kaybedersiniz.
- Kuruluşlar, Azure AD'de gibi ek güvenlik için multi-Factor authentication'ı etkinleştirmek için kullanarak kimlik doğrulaması özelleştirebilirsiniz.

## <a name="resource-endpoints"></a>Kaynak uç noktası

Uygulama uç noktaları yoluyla düzlemleri erişin. İki düzlem için erişim denetimleri birbirinden bağımsız olarak çalışır. Bir anahtar kasasındaki anahtarları kullanmak için bir uygulama erişimi vermek için bir anahtar kasası Erişim İlkesi'ni kullanarak veri düzlemi erişimi vermez. Bir anahtar kasası özelliklerini ve etiketlerini kullanıcı okuma erişimi ancak verilerini (anahtarlar, parola veya sertifikalara) erişimi vermek için RBAC ile yönetim düzlemi erişimi verin.

Aşağıdaki tabloda yönetim ve veri düzlemi uç noktaları gösterilmektedir.

| Erişim&nbsp;düzlemi | Erişim uç noktaları | İşlemler | Erişim&nbsp;denetim mekanizması |
| --- | --- | --- | --- |
| Yönetim düzlemi | **Genel:**<br> management.azure.com:443<br><br> **Azure Çin 21Vianet:**<br> management.chinacloudapi.cn:443<br><br> **Azure ABD:**<br> management.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> management.microsoftazure.de:443 | Oluşturun, okuyun, güncelleştirin ve anahtar kasası silme<br><br>Key Vault erişim ilkeleri ayarlama<br><br>Anahtar kasası etiketlerini ayarlama | Azure Resource Manager RBAC |
| Veri düzlemi | **Genel:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Azure Çin 21Vianet:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure ABD:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 | Anahtarlar: şifre çöz, şifrele,<br> çözme, sarmalama, doğrulayın, oturum,<br> Al, Listele, güncelleştirebilir, oluşturabilir,<br> İçeri Aktar, Sil, yedekleme, geri yükleme<br><br> Gizli diziler: almak, Listele, ayarla, Sil | Anahtar kasası erişim ilkesi |

## <a name="management-plane-and-rbac"></a>Yönetim düzlemi ve RBAC

Yönetim düzlemi işlemleri çağıran yürütmek yetkilendirmek için RBAC (rol tabanlı erişim denetimi) kullanın. RBAC modelinde, her Azure aboneliği bir Azure AD örneğini sahiptir. Kullanıcılar, gruplar ve uygulamalar için bu dizindeki erişim. Azure aboneliğinde Azure Resource Manager dağıtım modelini kullanan kaynakları yönetmek için erişim izni verilir. Erişim vermek [Azure portalında](https://portal.azure.com/), [Azure CLI](../cli-install-nodejs.md), [Azure PowerShell](/powershell/azureps-cmdlets-docs), veya [Azure Resource Manager REST API'leri](https://msdn.microsoft.com/library/azure/dn906885.aspx).

Bir kaynak grubunda bir anahtar kasası oluşturur ve Azure AD'yi kullanarak erişimi yönetin. Kullanıcılara vermek ya da bir kaynak grubundaki anahtar kasalarını yönetme olanağı gruplandırır. Belirli kapsam düzeyinde uygun RBAC rollerini atayarak erişim. Bir kullanıcıya anahtar kasalarını yönetmesi için erişim vermek için önceden tanımlanmış bir atamanız `key vault Contributor` belirli bir kapsamda kullanıcı rolü. Bir RBAC rolü için aşağıdaki kapsamları düzeyleri atanabilir:

- **Abonelik**: Abonelik düzeyinde atanmış bir RBAC rolü, tüm kaynak grupları ve bu Abonelikteki kaynaklar için geçerlidir.
- **Kaynak grubu**: Kaynak grubu düzeyinde atanmış bir RBAC rolü, kaynak grubundaki tüm kaynaklar için geçerlidir.
- **Belirli kaynak**: Belirli bir kaynağa atanmış bir RBAC rolü bu kaynak için geçerli olur. Bu durumda, belirli bir anahtar kasası kaynaktır.

Önceden tanımlı birkaç rol vardır. Önceden tanımlanmış bir role gereksinimlerinize uymuyorsa, kendi rolünüzü tanımlayabilirsiniz. Daha fazla bilgi için [RBAC: Yerleşik roller](../role-based-access-control/built-in-roles.md).

> [!IMPORTANT]
> Bir kullanıcı varsa `Contributor` bir anahtar kasası yönetim düzleminde, kullanıcı izinler kendilerini veri düzlemine erişim anahtar kasası erişim ilkesini ayarlayarak. Kimlerin sıkı bir şekilde denetlemesi gerekir `Contributor` anahtar kasalarınıza erişimi rolü. Yalnızca yetkili kişiler erişebilir ve anahtar kasası, anahtarlara, parolalara ve sertifikalara yönetme emin olun.
>

<a id="data-plane-access-control"></a> 
## <a name="data-plane-and-access-policies"></a>Veri düzlemi ve erişim ilkeleri

Key vault için anahtar kasası erişim ilkeleri ayarlanarak, veri düzlemi erişimi vermez. Bu erişim ilkeleri ayarlama, bir kullanıcı, Grup veya uygulama zorunda `Contributor` ilgili anahtar kasasının yönetim düzleminde izinleri.

Bir kullanıcı, Grup veya uygulama erişimini bir anahtar kasasındaki anahtarlar veya parolalar için belirli işlemleri yürütmek için vermesi. Key Vault için anahtar kasası erişim ilkesi girdileri en fazla 1024 destekler. Birkaç kullanıcıya veri düzlemi erişimi vermek için bir Azure AD güvenlik grubu oluşturun ve bu gruba kullanıcılar ekleyin.

<a id="key-vault-access-policies"></a> Key Vault erişim ilkeleri anahtarlara, parolalara ve sertifika için ayrı ayrı izinler verir. Yalnızca anahtarları ve gizli diziler için bir kullanıcı erişimi verebilirsiniz. Anahtarlara, parolalara ve sertifikalara erişim izinlerini kasa düzeyinde verilir. Key Vault erişim ilkeleri, bir özel anahtar, gizli veya sertifika gibi ayrıntılı, nesne düzeyinde izinleri desteklemez. Bir anahtar kasası için erişim ilkeleri ayarlamak için kullanın [Azure portalında](https://portal.azure.com/), [Azure CLI](../cli-install-nodejs.md), [Azure PowerShell](/powershell/azureps-cmdlets-docs), veya [anahtar kasası yönetim REST API'leri](https://msdn.microsoft.com/library/azure/mt620024.aspx).

> [!IMPORTANT]
> Anahtar kasası erişim ilkelerinin kasa düzeyinde uygulanır. Bir kullanıcı oluşturma ve silme izni verildiğinde kullanıcılar ilgili anahtar kasasındaki tüm anahtarlar üzerinde bu işlemleri gerçekleştirebilir.
>

Kullanarak veri düzlemi erişimi kısıtlayabilirsiniz [sanal ağ hizmet uç noktaları Azure Key Vault için](key-vault-overview-vnet-service-endpoints.md). Yapılandırabileceğiniz [güvenlik duvarları ve sanal ağ kuralları](key-vault-network-security.md) ek bir güvenlik katmanı için.

## <a name="example"></a>Örnek

Bu örnekte biz SSL, veri ve imzalama işlemleri için RSA 2.048 bit anahtarı depolamak için Azure depolama için bir sertifika kullanan bir uygulama geliştiriyor. Uygulamamız Azure sanal makine'de (VM) (veya bir sanal makine ölçek kümesi) çalıştırır. Uygulama parolalarını depolamak için bir anahtar kasası kullanabiliriz. Biz, uygulamanın Azure AD kimlik doğrulaması için kullandığı önyükleme sertifikasını depolayabilirsiniz.

Biz aşağıdaki depolanan anahtarları ve gizli anahtarları erişim gerekir:
- **SSL sertifikası**: SSL için kullanılır.
- **Depolama anahtarı**: Depolama hesabına erişmek için kullanılır.
- **RSA 2.048 bit anahtar**: İmzalama işlemleri için kullanılır.
- **Önyükleme sertifikası**: Azure AD kimlik doğrulaması için kullanılır. Erişimi verildikten sonra biz depolama anahtarını getirmek ve imzalama için RSA anahtarını kullanın.

Kimin yönetme, dağıtma ve uygulamamızı denetim belirtmek için aşağıdaki rolleri tanımlamak gerekir:
- **Güvenlik ekibi**: Office CSO (güvenlik Müdürü Yürüt) veya benzer katkıda bulunanlar BT personelidir. Güvenlik ekibi için gizli anahtarlarının doğru korunmasından sorumlu değildir. Gizli dizileri, SSL sertifikaları, imzalama, bağlantı dizeleri ve depolama hesabı anahtarları için RSA anahtarları içerebilir.
- **Geliştiriciler ve işleçler**: Uygulamayı geliştirip Azure'da dağıtan çalışanları. Bu takım üyelerinin güvenlik ekibi dahil değildir. Bunlar, SSL sertifikaları ve RSA anahtarları gibi hassas verilere erişimi olmamalıdır. Yalnızca dağıttıkları uygulama hassas verilere erişimi olmalıdır.
- **Denetçiler**: Bu rolü üyeleri geliştirme veya genel BT personelinden olmayan katkıda bulunanlar için kullanılır. Bunlar, kullanım ve sertifikaları, anahtarları ve parolaları güvenlik standartlarıyla uyumluluğu sağlamak için bakım gözden geçirin. 

Uygulamamızı kapsamı dışında başka bir rol yoktur: Abonelik (veya kaynak grubu) Yöneticisi. Abonelik Yöneticisi güvenlik ekibine ilk erişim izinlerini ayarlar. Bunlar, uygulama tarafından gerekli kaynaklara sahip bir kaynak grubu kullanarak güvenlik ekibine erişim verin.

Şu işlemler bizim rolleri için yetkilendirmek üzere gerekir:

**Güvenlik ekibi**
- Anahtar kasaları oluşturun.
- Anahtar kasası günlüğü etkinleştirin.
- Anahtarları ve gizli anahtarları ekleyin.
- Olağanüstü durum kurtarma için anahtarların yedeklemeler oluşturun.
- Kullanıcılara ve uygulamalara belirli işlemleri için izinleri vermek için Key Vault erişim ilkeleri ayarlayın.
- Anahtarları ve gizli anahtarları düzenli aralıklarla alma.

**Geliştiriciler ve işleçler**
- İmzalama için Güvenlik ekibinden önyükleme ve SSL sertifikalarının (parmak izleri), depolama anahtarının (gizli anahtar URI'si) ve RSA anahtar (anahtar URI'si) başvurularını alma.
- Geliştirin ve anahtarları ve gizli anahtarları programlı olarak erişmek için uygulamayı dağıtın.

**Denetçiler**
- Doğru kullanımı anahtarlara ve parolalara ve veri güvenliği standartlarına uyumu onaylamak için anahtar kasası günlükleri gözden geçirin.

Aşağıdaki tabloda rolleri ve uygulama erişim izinleri özetler. 

| Rol | Yönetim düzlemi izinleri | Veri düzlemi izinleri |
| --- | --- | --- |
| Güvenlik ekibi | Key Vault katkıda bulunanı | Anahtarlar: yedekleme, oluşturma, silme, alma, içeri aktarma, listeleme, geri yükleme<br>Gizli diziler: tüm işlemler |
| Geliştiriciler ve&nbsp;işleçleri | Anahtar kasası dağıtma izni<br><br> **Not**: Dağıtılan VM'lerin parolaları anahtar kasasından almak bu izin verir. | None |
| Denetçiler | None | Anahtarlar: listeleme<br>Parolalar: listeleme<br><br> **Not**: Bu izni, anahtarları ve gizli anahtarları günlüklerde gösterilen değil için (etiketler, etkinleştirme tarihler, sona erme tarihleri) özniteliklerini incelemek denetçiler sağlar. |
| Uygulama | None | Anahtarlar: imzalama<br>Parolalar: imzalama |

Üç takım rollerini Key Vault izinlerini yanı sıra diğer kaynaklara erişim gerekir. Sanal makineleri (veya Azure App Service'in Web Apps özelliği) dağıtmak için geliştiricilere ve işleçler gerek `Contributor` bu kaynak türlerine erişim. Denetçiler, anahtar kasası günlüklerinin depolandığı depolama hesabına Okuma erişimine ihtiyacı vardır.

Sertifikaları dağıtma hakkında daha fazla bilgi için erişim anahtarları ve gizli anahtarları programlı olarak şu kaynaklara bakın:
- Bilgi nasıl [sertifikaları, müşteri tarafından yönetilen bir anahtar kasasından Vm'lerine dağıtma](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) (blog gönderisi).
- İndirme [Azure anahtar kasası istemci örnekleri](https://www.microsoft.com/download/details.aspx?id=45343). Bu içerik, önyükleme sertifikası bir anahtar kasasına erişmek için Azure AD kimlik doğrulaması için nasıl kullanılacağı gösterilmektedir.

Azure portalını kullanarak çoğu erişim izinleri verebilirsiniz. Ayrıntılı izinler vermek için Azure PowerShell veya Azure CLI'yı kullanabilirsiniz.

Bu bölümdeki PowerShell kod parçacıkları şu varsayımlara ile oluşturulur:
- Azure AD Yöneticisi, üç rolü temsil eden güvenlik grupları oluşturmuştur: Contoso güvenlik ekibi, Contoso uygulama DevOps ve Contoso uygulama denetçileri. Yönetici kullanıcılar, ilgili gruplarına eklemiştir.
- Tüm kaynaklar bulunan **ContosoAppRG** kaynak grubu.
- Anahtar kasası günlükleri depolanan **contosologstorage** depolama hesabı. 
- **ContosoKeyVault** anahtar kasası ve **contosologstorage** depolama hesabı aynı Azure konumunda olan.

Abonelik Yöneticisi atar `key vault Contributor` ve `User Access Administrator` güvenlik ekibine rolleri. Bu roller, güvenlik ekibinin diğer kaynaklara ve anahtar kasalarına erişimi yönetmek izin ver, her ikisi de **ContosoAppRG** kaynak grubu.

```powershell
New-AzRoleAssignment -ObjectId (Get-AzADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzRoleAssignment -ObjectId (Get-AzADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Güvenlik ekibinin anahtar kasası oluşturulur ve günlüğe kaydetme ve erişim izinlerini ayarlar. Anahtar kasası erişim ilkesi izinleri hakkında daha fazla ayrıntı için bkz: [Azure Key Vault hakkında anahtarlara, parolalara ve sertifikalara](about-keys-secrets-and-certificates.md).

```powershell
# Create a key vault and enable logging
$sa = Get-AzStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzKeyVault -Name ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent

# Set up data plane permissions for the Contoso Security Team role
Set-AzKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge

# Set up management plane permissions for the Contoso App DevOps role
# Create the new role from an existing role
$devopsrole = Get-AzRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App DevOps"
$devopsrole.Description = "Can deploy VMs that need secrets from a key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permissions for the Contoso App DevOps role so members can deploy VMs with secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzRoleDefinition -Role $devopsrole

# Assign the new role to the Contoso App DevOps security group
New-AzRoleAssignment -ObjectId (Get-AzADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Set up data plane permissions for the Contoso App Auditors role
Set-AzKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

Bizim tanımlanmış özel roller yalnızca aboneliğe atanabilir burada **ContosoAppRG** kaynak grubu oluşturulur. Diğer Aboneliklerdeki diğer projeler için özel bir rol kullanmak için rolü için kapsamı diğer Aboneliklerdeki ekleyin.

Anahtar kasası için özel bir rol ataması bizim DevOps personeli `deploy/action` kaynak grubuna izin kapsamı belirlenmiş. Oluşturulan VM'ler **ContosoAppRG** kaynak grubu, parolaları (SSL ve önyükleme sertifikaları) için erişim verilir. DevOps üyesi tarafından diğer kaynak grubunda oluşturulan VM'ler, gizli bir URI'leri VM'ye sahip olsa bile bu parolaları erişemez.

Bizim örneğimizde, basit bir senaryoyu açıklar. Gerçek yaşam senaryoları daha karmaşık olabilir. Anahtar kasanıza gereksinimlerinize göre izinleri ayarlayabilirsiniz. Biz kabul, güvenlik ekibinin uygulamalarında DevOps personeli tarafından kullanılan parola başvurularını (URI'ler ve parmak izleri) ve anahtar sağlar. Geliştiriciler ve işleçleri herhangi bir veri düzlemi erişimi gerekli değildir. Biz, anahtar kasanızın güvenliğini sağlama konusunda odaklanır. Benzer göz önünde bulundurarak, güvenli hale getirdiğinizde vermek [Vm'lerinizi](https://azure.microsoft.com/services/virtual-machines/security/), [depolama hesapları](../storage/common/storage-security-guide.md)ve diğer Azure kaynakları.

> [!NOTE]
> Bu örnek nasıl anahtar kasası erişim üretimde kilitli gösterir. Geliştiriciler uygulamayı geliştirdikleri burada kendi abonelik veya kaynak grubu, kasalar, VM'ler ve depolama hesabını yönetmek için tam izinlere sahip olması gerekir.

Anahtar kasanıza ek güvenli erişimi ayarlama öneririz [Key Vault güvenlik duvarları ve sanal ağları yapılandırma](key-vault-network-security.md).

## <a name="resources"></a>Kaynaklar

* [Azure AD RBAC](../role-based-access-control/role-assignments-portal.md)

* [RBAC: Yerleşik roller](../role-based-access-control/built-in-roles.md)

* [Resource Manager dağıtımını ve klasik dağıtımı anlama](../azure-resource-manager/resource-manager-deployment-model.md) 

* [Azure PowerShell ile RBAC yönetme](../role-based-access-control/role-assignments-powershell.md) 

* [REST API ile RBAC yönetme](../role-based-access-control/role-assignments-rest.md)

* [Microsoft Azure için RBAC](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Bu bir 2015 Microsoft video konferans anlatılmaktadır Ignite yönetim ve raporlama özellikleri erişin. Ayrıca, Azure AD'yi kullanarak Azure aboneliklerine erişimin güvenliğini sağlamak için en iyi uygulamaları açıklar.

* [OAuth 2.0 ve Azure AD kullanarak web uygulamalarına erişim yetkisi verme](../active-directory/develop/v1-protocols-oauth-code.md)

* [Anahtar kasası yönetim REST API'leri](https://msdn.microsoft.com/library/azure/mt620024.aspx)

    Anahtarınızı yönetmek REST API başvurusunu kasası programlı olarak, anahtar kasası erişim ilkesinin ayarlanması dahil olmak üzere.

* [Anahtar kasası REST API'leri](https://msdn.microsoft.com/library/azure/dn903609.aspx)

* [Anahtar erişim denetimi](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
* [Gizli anahtar erişim denetimi](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
* [Ayarlama](/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy) ve [Kaldır](/powershell/module/az.keyvault/Remove-azKeyVaultAccessPolicy) PowerShell kullanarak anahtar kasası erişim ilkesi.
  
## <a name="next-steps"></a>Sonraki adımlar

Yapılandırma [Key Vault güvenlik duvarları ve sanal ağlar](key-vault-network-security.md).

Bir yöneticinin başlangıç öğretici için bkz. [Azure anahtar kasası nedir?](key-vault-overview.md).

Key Vault kullanım günlüğü hakkında daha fazla bilgi için bkz. [Azure Key Vault günlüğü](key-vault-logging.md).

Azure anahtar kasası ile anahtarları ve gizli anahtarları kullanma hakkında daha fazla bilgi için bkz. [anahtarlar ve gizlilikler hakkında](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Anahtar kasası ile ilgili sorularınız varsa bkz [forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
