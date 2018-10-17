---
title: Azure anahtar kasanızın güvenliğini sağlama | Microsoft Docs
description: Azure Key Vault, anahtarları ve parolaları yönetmek için anahtar kasası için erişim izinlerini yönetin. Key vault ile anahtar kasanızın güvenliğini sağlama için kimlik doğrulama ve yetkilendirme modeli.
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
ms.openlocfilehash: 4a1de3c011f1f8cfa1ea9246efad4ebb7f9e3a76
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364532"
---
# <a name="secure-your-key-vault"></a>Anahtar kasanızın güvenliğini sağlama
Azure Key Vault, şifreleme anahtarlarını ve gizli anahtarları (örneğin, sertifikalar, bağlantı dizeleri, parolalar gibi) koruyan bir bulut hizmetidir. Bu veri büyük/küçük harfe duyarlıdır ve iş açısından kritik, anahtar kasalarınıza erişimi güvenli hale izin vererek yalnızca uygulamaların ve kullanıcıların erişim elde etmek için yetkili. 

Bu makalede anahtar kasası erişim modeline genel bakış sağlar. Kimlik doğrulama ve yetkilendirme açıklanmakta ve anahtar kasasına erişim güvenliğinin nasıl sağlanacağını açıklar.

## <a name="overview"></a>Genel Bakış
Bir anahtar kasasına erişim, iki ayrı arabirim ile denetlenir: yönetim düzlemi ve veri düzlemi. Bir çağıranın (kullanıcı ya da uygulama) anahtar kasasına erişim elde edebilmesi her iki düzlem için uygun kimlik doğrulaması ve yetkilendirme gerekli değildir. Kimlik doğrulaması, yetkilendirme çağıranın yapmaya izinli işlemleri belirler arayan kimliğini oluşturur.

Kimlik doğrulaması için yönetim düzlemi ve veri düzleminde Azure Active Directory kullanılır. Yetkilendirme için yönetim düzleminde rol tabanlı erişim denetimi (RBAC), veri düzleminde ise anahtar kasası erişim ilkesi kullanılır.

Ele alınan konulara kısa bir genel bakış aşağıda verilmiştir:

[Azure Active Directory kullanarak kimlik doğrulaması](#authentication-using-azure-active-directory) - Bu bölümde bir çağıranın anahtar kasasına yönetim düzlemi ve veri düzlemi üzerinden erişmek için Azure Active Directory ile kimlik doğrulaması yapma işlemi açıklanmaktadır. 

[Yönetim düzlemi ve veri düzlemi](#management-plane-and-data-plane) - Yönetim düzlemi ve veri düzlemi, anahtar kasanıza erişmek için kullanılan iki erişim düzlemidir. Her erişim düzlemi belirli işlemleri destekler. Bu bölümde erişim uç noktaları, desteklenen işlemler ve her bir düzlem tarafından kullanılan erişim denetimi açıklanmaktadır. 

[Yönetim düzlemi erişim denetimi](#management-plane-access-control) - Bu bölümde rol tabanlı erişim denetimi kullanılarak yönetim düzlemi işlemlerine erişim izni verme konusu ele alınacaktır.

[Veri düzlemi erişim denetimi](#data-plane-access-control) - Bu bölümde veri düzlemi erişimini denetlemek için anahtar kasası erişim ilkesini kullanma işlemi açıklanmaktadır.

[Örnek](#example) -Bu örnekte, üç farklı ekibin (güvenlik ekibi, geliştiriciler/operatörler ve denetçiler) geliştirmek, yönetmek ve azure'da bir uygulamayı izlemek üzere belirli görevleri gerçekleştirmesine izin vermek anahtar kasanız için erişim denetimini ayarlama işlemi açıklanmaktadır.

## <a name="authentication-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak kimlik doğrulama
Bir Azure aboneliğinde bir anahtar kasası oluşturduğunuzda, aboneliğin Azure Active Directory kiracısı ile otomatik olarak ilişkilendirilir. Tüm çağıranların (kullanıcılar ve uygulamalar) bu kiracıya kaydedilmesi gerekir ve anahtar kasası erişim için kimlik doğrulaması gerekir. Bu gereksinim, her iki Yönetim düzlemi ve veri düzlemi erişimi için geçerlidir. Her iki durumda da bir uygulama, anahtar kasasına iki şekilde erişebilir:

* **Kullanıcı + uygulama erişimi** - oturum açmış kullanıcı adına anahtar kasasına erişen uygulamalar ile kullanılır. Azure PowerShell ve Azure portalı bu tür bir erişimin örnekleridir. Kullanıcılara erişim vermek için iki yolu vardır: 
- Herhangi bir uygulamadan anahtar kasasına erişebilmesi için bu kullanıcılara vermek erişim.
- Yalnızca (Birleşik kimlik olarak adlandırılır) belirli bir uygulama kullandığınızda, bir kullanıcının anahtar kasasına erişim verin.

* **Salt uygulama erişim** - daemon Hizmetleri çalıştırmasına veya arka plan işleri uygulamalarla birlikte kullanılır. Uygulamanın kimliğine anahtar kasası erişimi verilir.

Her iki uygulama türünde de, [desteklenen kimlik doğrulama yöntemlerinden](../active-directory/develop/authentication-scenarios.md) biri kullanılarak uygulamanın Azure Active Directory kimlik doğrulaması yapılır ve uygulama bir belirteç alır. Kullanılan kimlik doğrulama yöntemi, uygulama türüne bağlıdır. Uygulama bundan sonra bu belirteci kullanıcı ve anahtar kasasına REST API isteği gönderir. Yönetim düzlemi istekleri, bir Azure Resource Manager uç noktasından yönlendirilir. Veri düzlemine erişirken uygulamalar bir anahtar kasası uç noktasıyla doğrudan konuşur. [Kimlik doğrulama akışının tamamı](../active-directory/develop/v1-protocols-oauth-code.md) hakkında daha fazla bilgi edinin. 

Uygulamanın belirteç istediği kaynak adı, uygulamanın yönetim düzlemine veya veri düzlemine erişmesine bağlı olarak değişir. Bu nedenle kaynak adı, Azure ortamına bağlı olarak sonraki bir bölümde bulunan tablodaki yönetim düzlemi veya veri düzlemi uç noktasıdır.

Hem yönetim hem de veri düzleminde kimlik doğrulaması için tek bir mekanizmaya sahip olunması kendi avantajlarına sahiptir:

* Kuruluşlar, kuruluş içindeki tüm anahtar kasalarına erişimi merkezi olarak denetleyebilir
* Bir kullanıcı ayrılırsa, kuruluştaki tüm anahtar kasalarına erişimini kaybeder
* Kuruluşlar Azure Active Directory’deki seçenekleri kullanarak kimlik doğrulamasını özelleştirebilir (örneğin, daha fazla güvenlik için çok faktörlü kimlik doğrulamasını etkinleştirme)

## <a name="management-plane-and-data-plane"></a>Yönetim düzlemi ve veri düzlemi
Azure Key Vault, Azure Resource Manager dağıtım modeli üzerinden kullanılabilen bir Azure hizmeti var. Bir anahtar kasası oluşturduğunuzda, anahtarlara, parolalara ve sertifikalara gibi hassas nesneleri depolamak için sanal bir kapsayıcı alın. Belirli işlemler, yönetim düzlemi ve veri düzlemi arabirimlerine kullanarak anahtar kasası üzerinde gerçekleştirilir. Yönetim düzlemi, anahtar kasasını yönetmek için kullanılır. Bu öznitelikler yönetme ve veri düzlemi erişim ilkelerini ayarlama gibi işlemleri içerir. Veri düzlemi arabirimi eklemek, silmek, değiştirmek ve anahtarlara, parolalara ve anahtar kasasında depolanan sertifikaları kullanmak için kullanılır.

Yönetim düzlemi ve veri düzlemi arabirimlerine farklı uç noktalar aşağıda listelenen aracılığıyla erişilir. İkinci sütunda farklı Azure ortamlarında bu uç noktaların DNS adlarını tanımlar. Üçüncü sütunda, her erişim düzleminde gerçekleştirebileceğiniz işlemler açıklanır. Her erişim düzlemi kendi erişim denetimi mekanizmasını de vardır. Yönetim düzlemi erişim denetimi Azure Resource Manager Role-Based erişim denetimi (RBAC) kullanılarak ayarlanır. Veri düzlemi erişim denetimi, anahtar kasası erişim ilkesi kullanılarak ayarlanır.

| Erişim düzlemi | Erişim uç noktaları | İşlemler | Erişim denetimi mekanizması |
| --- | --- | --- | --- |
| Yönetim düzlemi |**Genel:**<br> management.azure.com:443<br><br> **Azure Çin 21Vianet:**<br> management.chinacloudapi.cn:443<br><br> **Azure ABD:**<br> management.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> management.microsoftazure.de:443 |Anahtar kasası oluşturma/okuma/güncelleştirme/silme <br> Anahtar kasası için erişim ilkelerini ayarlama<br>Anahtar kasası etiketlerini ayarlama |Azure Resource Manager Rol Tabanlı Access Control (RBAC) |
| Veri düzlemi |**Genel:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Azure Çin 21Vianet:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure ABD:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |Anahtarlar için: Şifre Çöz, Şifrele, UnwrapKey, WrapKey, Doğrula, İmzala, Al, Listele, Güncelleştir, Oluştur, İçeri Aktar, Sil, Yedekle, Geri Yükle<br><br> Parolalar için: Al, Listele, Ayarla, Sil |Anahtar kasası erişim ilkesi |

Yönetim düzlemi ve veri düzlemi erişim denetimleri birbirinden bağımsız olarak çalışır. Bir anahtar kasasındaki anahtarları kullanmak için bir uygulama erişim vermek istiyorsanız, örneğin, veri düzlemi erişimi vermek yeterlidir. Key vault erişim ilkeleri erişim izni verilir. Buna karşılık, kasa özellikleri ve etiketler, ancak değil erişim verilerini (anahtarlar, parola veya sertifikalara) okumak için gereken bir kullanıcının yalnızca denetim düzlemi erişimi gerekir. 'RBAC ile kullanıcılara Okuma erişimi' atayarak erişim izni verilir.

## <a name="management-plane-access-control"></a>Yönetim düzlemi erişim denetimi
Yönetim düzlemi, anahtar kasası kendisi gibi etkileyen işlemlerden oluşur:

- Oluşturma veya bir anahtar kasası siliniyor.
- Bir Abonelikteki kasaların listesini alınıyor.
- Anahtar kasası özelliklerini (örneğin, SKU, etiketler) alınıyor.
- Anahtarları ve gizli anahtarları için denetimi kullanıcı ve uygulama erişim key vault erişim ilkeleri ayarlama.

Yönetim düzlemi erişim denetimi, RBAC kullanır. Önceki bölümde yer tablodaki yönetim düzlemi üzerinden gerçekleştirilebilecek anahtar kasası işlemlerinin tam listesine bakın. 

### <a name="role-based-access-control-rbac"></a>Rol Tabanlı Access Control (RBAC)
Her Azure aboneliği bir Azure Active Directory’ye sahiptir. Bu dizindeki kullanıcılara, gruplara ve uygulamalara, Azure aboneliğinde Azure Resource Manager dağıtım modelini kullanan kaynakları yönetmek üzere erişim verilebilir. Bu erişim denetimi türü, Rol Tabanlı Access Control (RBAC) olarak adlandırılır. Bu erişimi yönetmek için [Azure portalı](https://portal.azure.com/), [Azure CLI araçları](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) veya [Azure Resource Manager REST API'lerini](https://msdn.microsoft.com/library/azure/dn906885.aspx) kullanabilirsiniz.

Bir anahtar kasası kaynak grubu ve Denetim erişimi Azure Active Directory'yi kullanarak yönetim düzlemine oluşturun. Örneğin, kullanıcı veya grup kaynak grubundaki anahtar kasalarını yönetme olanağı verebilirsiniz.

Uygun RBAC rollerini atayarak erişim için kullanıcılara, gruplara ve uygulamalara belirli bir kapsamda verebilirsiniz. Örneğin, bir kullanıcıya anahtar kasalarını yönetmesi için erişim vermek için önceden tanımlanmış bir 'Key Vault katkıda bulunanı' rolü belirli bir kapsamda bu kullanıcıya atayın. Kapsam, bir abonelik, kaynak grubu veya belirli bir anahtar kasası bu durumda olacaktır. Abonelik düzeyinde atanmış bir rol, bu abonelikteki tüm kaynak grupları ve kaynaklar için geçerlidir. Kaynak grubu düzeyinde atanmış bir rol, ilgili kaynak grubundaki tüm kaynaklar için geçerli olur. Belirli bir kaynağa atanmış bir rol, ilgili kaynak için geçerlidir. Önceden tanımlı birkaç rol vardır (bkz [RBAC: yerleşik roller](../role-based-access-control/built-in-roles.md)). Önceden tanımlanmış bir role gereksinimlerinize uymuyorsa, kendi rolünüzü tanımlayabilirsiniz.

> [!IMPORTANT]
> Bir kullanıcı bir anahtar kasası yönetim düzleminde Katkıda Bulunan izinlerine (RBAC) sahipse, veri düzlemine erişimi denetleyen anahtar kasası erişim ilkesini ayarlayarak, kendisine veri düzlemine erişim verebilir. Bu nedenle, anahtar kasalarınıza, anahtarlarınıza, parolalarınıza ve sertifikalarınıza erişip bunları yönetebilen kullanıcıların yalnızca yetkili kişiler olduğundan emin olmak amacıyla anahtar kasalarınıza 'Katkıda Bulunan' erişimine kimlerin sahip olduğunun sıkı denetlenmesi önerilir.
> 
> 

## <a name="data-plane-access-control"></a>Veri düzlemi erişim denetimi
Anahtar kasası veri düzlemi işlemleri, anahtarlara, parolalara ve sertifikalara gibi depolanan nesnelere uygulanır. Anahtar işlemleri oluşturma, alma, güncelleştirme, liste, yedekleme ve geri yükleme anahtarları. Şifreleme işlemleri oturum dahil, doğrulayın, şifreleme, şifresini çözmek, sarmalama, sarmalamadan çıkarma, etiketler ve diğer özniteliklerini ayarlayın. Benzer şekilde, gizli diziler üzerinde işlemler dahil get, set, liste silin.

Veri düzlemi erişimi, bir anahtar kasasına ilişkin erişim ilkeleri ayarlanarak verilir. Bir kullanıcı, grup veya uygulamanın bir anahtar kasasına ait erişim ilkelerini ayarlayabilmesi için, ilgili anahtar kasasının yönetim düzleminde Katkıda Bulunan izinlerine (RBAC) sahip olması gerekir. Bir kullanıcı, grup veya uygulamaya, bir anahtar kasasındaki anahtarlar veya parolalar için belirli işlemleri gerçekleştirmek üzere erişim verilebilir. Key Vault, bir anahtar kasası için en fazla 1024 adet erişim ilkesi girişi destekler. Bir Azure Active Directory güvenlik grubu oluşturun ve bir anahtar kasasındaki birkaç kullanıcıya veri düzlemi erişimi vermek amacıyla ilgili gruba kullanıcılar ekleyin.

### <a name="key-vault-access-policies"></a>Key Vault Erişim İlkeleri
Key vault erişim ilkeleri anahtarlara, parolalara ve sertifikalara ayrı ayrı izinler. Örneğin, yalnızca anahtarlar için kullanıcı erişim ve gizlilik için herhangi bir izin verebilirsiniz. Anahtar, parola veya sertifikalara erişim izni kasa düzeyinde verilir. Anahtar kasası erişim ilkesi, bir özel anahtar/gizli diziniz/sertifikanız gibi ayrıntılı nesne düzeyinde izinleri desteklemez. Bir anahtar kasasının erişim ilkelerini ayarlamak için [Azure portalı](https://portal.azure.com/), [Azure CLI araçları](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) veya [anahtar kasası Yönetim REST API’lerini](https://msdn.microsoft.com/library/azure/mt620024.aspx) kullanabilirsiniz.

> [!IMPORTANT]
> Anahtar kasası erişim ilkelerinin kasa düzeyinde geçerli olduğunu unutmayın. Örneğin, bir kullanıcıya anahtar oluşturma ve silme izni verildiğinde kullanıcı bu işlemleri ilgili anahtar kasasındaki tüm anahtarlar üzerinde gerçekleştirebilir.

Erişim ilkelerinin yanı sıra, ek güvenlik katmanı için [Güvenlik duvarları ve sanal ağ kuralları](key-vault-network-security.md)’nı yapılandırılarak [Azure Key Vault için Sanal Ağ Hizmet Uç Noktaları](key-vault-overview-vnet-service-endpoints.md) ile veri düzlemi erişimi de kısıtlanabilir.

## <a name="example"></a>Örnek
Veri ve imzalama işlemleri için RSA 2048 bit anahtarı depolamak için bir SSL sertifikası, Azure depolama kullanan bir uygulama geliştirirken varsayalım. Bu uygulamayı çalıştıran bir Azure sanal makine (veya bir sanal makine ölçek kümesi) varsayalım. Tüm uygulama parolalarını depolamak için bir anahtar Kasası'nı kullanın ve Azure Active Directory ile kimlik doğrulaması yapmak için uygulama tarafından kullanılan önyükleme sertifikasını depolayabilirsiniz.

Anahtarlar ve gizli anahtar kasasında depolanan türlerinin bir özeti aşağıda verilmiştir:

* **SSL sertifikası** - SSL için kullanılır
* **Depolama anahtarı** - depolama hesabı erişim elde etmek için kullanılır
* **RSA 2048 bit anahtarı** - imzalama işlemleri için kullanılır
* **Önyükleme sertifikası** - Azure Active Directory ile kimlik doğrulaması için kullanılır. Erişim verildiğinde depolama anahtarını getirmek ve imzalama için RSA anahtarını kullanın.

Şimdi şimdi olanlar, yönetme, dağıtma ve bu uygulama denetimi kişilerle tanışalım. Bu örnekte üç rol kullanılacaktır.

* **Güvenlik ekibi** -'office' CSO (güvenlik Müdürü Yürüt), genellikle BT personelinden veya eşdeğer. Bu takım için gizli anahtarlarının doğru korunmasından sorumlu değildir. Örneğin, SSL sertifikaları, bağlantı dizeleri, imzalama için kullanılan RSA anahtarları ve depolama hesabı anahtarları.
* **Geliştiriciler/operatörler** -kimin uygulamayı geliştirip Azure'da dağıtan istedim. Genellikle, olmadıklarını güvenlik ekibinin bir parçası ve bu nedenle SSL sertifikaları ve RSA anahtarları gibi hassas verilere erişimi olmamalıdır. Yalnızca dağıttıkları uygulama bu nesnelere erişimi olmalıdır.
* **Denetçiler** -geliştiricileri ve genel BT personelinden ayrı genellikle farklı bir dizi. Kullanım ve sertifikaları, anahtarları ve parolaları güvenlik standartlarıyla uyumluluğu sağlamak için bakım gözden geçirmek için kendi sorumluluğudur. 

Kapsamı bu uygulama, ancak ilgili dışında burada bahsedilecek olan rol daha mevcuttur. Aboneliği (veya kaynak grubu) bu rolü olan yönetici. Abonelik Yöneticisi güvenlik ekibine ilk erişim izinlerini ayarlar. Abonelik Yöneticisi, bu uygulama için gereken kaynakları içeren kaynak grubunu kullana güvenlik ekibine erişim verir.

Her bir rolün bu uygulama bağlamında gerçekleştirdiği işlemler aşağıda verilmiştir.

* **Güvenlik ekibi**
  * Anahtar kasaları oluşturma
  * Anahtar kasası günlüğünü açma
  * Anahtar/parola ekleme
  * Olağanüstü durum kurtarma için anahtarların yedeğini oluşturma
  * Kullanıcılara ve uygulamalara belirli işlemleri gerçekleştirmesi için izinler veren anahtar kasası erişim ilkesini ayarlama
  * Anahtarları/parolaları düzenli aralıklarla alma
* **Geliştiriciler/operatörler**
  * Güvenlik ekibinden önyükleme ve SSL sertifikalarının (parmak izleri), depolama anahtarının (URI) ve imzalama anahtarının (anahtar URI'si) başvurularını alma
  * Anahtarlara ve parolalara programlı olarak erişen uygulamayı geliştirme ve dağıtma
* **Denetçiler**
  * Uygun anahtar/parola kullanımını ve veri güvenliği standartlarına uyumu onaylamak üzere kullanım günlüklerini gözden geçirme

Artık her rol ve uygulamanın atanmış görevleri gerçekleştirmesi için gerekli erişim izinlerinin ne görelim. 

| Kullanıcı Rolü | Yönetim düzlemi izinleri | Veri düzlemi izinleri |
| --- | --- | --- |
| Güvenlik Ekibi |anahtar kasasına Katkıda Bulunan |Anahtarlar: yedekleme, oluşturma, silme, alma, içeri aktarma, listeleme, geri yükleme <br> Parolalar: tümü |
| Geliştiriciler/Operatörler |dağıttıkları VM’lerin parolaları anahtar kasasından getirebilmesi anahtar kasası dağıtma izni |None |
| Denetçiler |None |Anahtarlar: listeleme<br>Parolalar: listeleme |
| Uygulama |None |Anahtarlar: imzalama<br>Parolalar: imzalama |

> [!NOTE]
> Denetçilerin etiketler, etkinleştirme ve sona erme tarihleri gibi günlüklerde verilmeyen anahtar ve parola özniteliklerini inceleyebilmesi için anahtar ve parolalara yönelik listeleme izni gereklidir.
> 
> 

Anahtar kasası izinlere ek olarak üç rolün tamamı da diğer kaynaklara erişim gerekir. Örneğin, VM’leri (veya Web Apps, vb.) dağıtabilmek için Geliştiriciler/Operatörler aynı zamanda bu kaynak türlerine 'Katkıda Bulunan' erişimine sahip olmalıdır. Denetçiler 'depolama hesabı anahtar kasası günlüklerinin depolandığı okuma erişimi '.

Bu makalenin odak noktası anahtar kasanıza erişimin güvenliğinin sağlanması olduğundan, biz yalnızca bu konuyla kavramları göstermektedir. Sertifikaları dağıtma, anahtarları ve gizli anahtarları program aracılığıyla erişme ve diğerleri ile ilgili ayrıntıları başka bir yerde ele alınmaktadır. Örneğin:

- VM'ler için key vault'ta depolanan sertifikaları dağıtma kapsanan bir [blog gönderisi: dağıtma müşteri tarafından yönetilen Key vault'tan VM'ler için sertifikalar](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)
- [Azure anahtar kasası istemci örnekleri indirin](https://www.microsoft.com/download/details.aspx?id=45343) bir önyükleme sertifikası bir anahtar kasasına erişmek için Azure AD kimlik doğrulaması için nasıl kullanılacağı gösterilmektedir.

Azure portalını kullanarak çoğu erişim izni verilebilir. Ayrıntılı izinler vermek için istenen sonucu elde etmek için Azure PowerShell veya CLI kullanın gerekebilir. 

Aşağıdaki PowerShell kod parçacıkları şu varsayımlara sahiptir:

* Azure Active Directory yöneticisi rolleri (Contoso güvenlik ekibi, Contoso uygulama Devops, Contoso uygulama denetçileri) temsil eden güvenlik grupları oluşturmuştur. Yönetici ayrıca ait oldukları gruplara kullanıcılar eklemiştir.
* **ContosoAppRG** tüm kaynakların bulunduğu kaynak grubudur. **contosologstorage**, günlüklerin depolandığı yerdir. 
* **ContosoKeyVault** anahtar kasası ve **contosologstorage** anahtar kasası günlükleri için kullanılan depolama hesabı aynı Azure konumunda olmalıdır

İlk olarak, abonelik yöneticisi güvenlik ekibine 'anahtar kasasına Katkıda Bulunan' ve 'Kullanıcı Erişimi Yöneticisi' rollerini atar. Bu roller, güvenlik ekibinin diğer kaynaklara erişimi yönetme ve ContosoAppRG kaynak grubundaki anahtar kasalarını yönetmenize izin verin.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Aşağıdaki komut dosyasını nasıl güvenlik ekibinin anahtar kasası oluşturabilir ve günlüğe kaydetme ve erişim izinleri ayarlama gösterir. Bkz: [Azure Key Vault hakkında anahtarlara, parolalara ve sertifikalara](about-keys-secrets-and-certificates.md) Key Vault hakkında ayrıntılı bilgi için erişim ilkesi izinleri.

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

Tanımlanan özel rol yalnızca ContosoAppRG kaynak grubunun oluşturulduğu aboneliğe atanabilir. Diğer Aboneliklerdeki diğer projeler için aynı özel roller kullanılacaksa, kapsamı daha fazla abonelik eklenebilir olabilir.

Geliştiriciler/operatörler "dağıtma/işlem" iznine yönelik özel rol ataması, kaynak grubu kapsamında yapılır. Bu, yalnızca parolaları (SSL sertifikası ve önyükleme sertifikası) erişimi için ' ContosoAppRG' kaynak grubunda oluşturulan VM'ler sağlar. Gizli bir URI'leri olsa bile dev/ops takım üyesi tarafından başka bir kaynak grubunda oluşturulan VM'ler bu gizli dizileri erişemezsiniz.

Bu örnek, basit bir senaryo gösterir. Gerçek yaşam senaryoları daha karmaşık olabilir ve gereksinimlerinize göre anahtar kasanıza izinleri ayarlamanız gerekebilir. Bu örnekte güvenlik ekibinin anahtar ve geliştiriciler/operatörler uygulamalarında başvurmaları gereken parola başvurularını (URI'ler ve parmak izleri) sağlayacak varsayılır. Geliştiriciler/operatörler herhangi bir veri düzlemi erişimi gerektirmez. Bu örnekte anahtar kasanızın güvenliğini sağlama üzerinde odaklanır. Benzer verilmelidir güvenliğini sağlamak için [Vm'lerinizi](https://azure.microsoft.com/services/virtual-machines/security/), [depolama hesapları](../storage/common/storage-security-guide.md)ve diğer Azure kaynakları.

> [!NOTE]
> Not: Bu örnekte, üretim sırasında anahtar kasası erişiminin nasıl kilitleneceği gösterilmektedir. Geliştiriciler uygulamayı geliştirdikleri kasalar, VM’ler ve depolama hesabını yönetmek için tam izinlerinin olduğu aboneliklere veya kaynak grubuna sahip olmalıdır.

Erişim sağlamak için anahtar kasanıza daha ayrıntılı önemle tavsiye edilir [Key Vault güvenlik duvarları ve sanal ağları yapılandırma](key-vault-network-security.md).

## <a name="resources"></a>Kaynaklar
* [Azure Active Directory Rol Tabanlı Erişim Denetimi](../role-based-access-control/role-assignments-portal.md)
  
  Bu makalede Azure Active Directory Rol Tabanlı Access Control ve nasıl çalıştığı açıklanmaktadır.
* [RBAC: Yerleşik Roller](../role-based-access-control/built-in-roles.md)
  
  Bu makalede RBAC’de kullanılabilen tüm yerleşik roller ayrıntılı olarak açıklanmaktadır.
* [Resource Manager dağıtımını ve klasik dağıtımı anlama](../azure-resource-manager/resource-manager-deployment-model.md)
  
  Bu makalede Resource Manager dağıtımı ve klasik dağıtım modelleri açıklanmakta ve Resource Manager ile kaynak gruplarını kullanmanın avantajları anlatılmaktadır
* [Rol Tabanlı Erişim Denetimini Azure PowerShell ile Yönetme](../role-based-access-control/role-assignments-powershell.md)
  
  Bu makalede rol tabanlı erişim denetiminin Azure PowerShell ile nasıl denetleneceği açıklanmaktadır
* [Rol Tabanlı Erişim Denetimini REST API’si ile Yönetme](../role-based-access-control/role-assignments-rest.md)
  
  Bu makalede RBAC yönetimi için REST API’sinin nasıl kullanılacağı gösterilmektedir.
* [Ignite’tan Microsoft Azure için Rol Tabanlı Erişim Denetimi](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  Bu bir 2015 Microsoft video konferans anlatılmaktadır Ignite yönetim ve raporlama özellikleri erişin. Ayrıca, Azure Active Directory kullanarak Azure aboneliklerine erişimin güvenliğini sağlamak için en iyi keşfediyor.
* [OAuth 2.0 ve Azure Active Directory kullanarak web uygulamalarına erişim yetkisi verme](../active-directory/develop/v1-protocols-oauth-code.md)
  
  Bu makalede Azure Active Directory ile kimlik doğrulaması için tam OAuth 2.0 akışı açıklanmaktadır.
* [anahtar kasası Yönetim REST API’leri](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Bu belge, anahtar kasası erişim ilkesinin ayarlanması dahil olmak üzere anahtar kasanızı programlı olarak yönetmeye yönelik REST API’leri başvurusudur.
* [anahtar kasası REST API’leri](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  Anahtar kasası REST API başvuru belgelerinin bağlantısı.
* [Anahtar erişim denetimi](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  Gizli anahtar erişim denetimi başvuru belgelerinin bağlantısı.
* [Gizli anahtar erişim denetimi](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  Anahtar erişim denetimi başvuru belgelerinin bağlantısı.
* PowerShell kullanarak anahtar kasası erişimini [ayarlama](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy) ve [kaldırma](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Remove-AzureRmKeyVaultAccessPolicy)
  
  Anahtar kasası erişim ilkesini yönetmeye yönelik PowerShell cmdlet’lerinin başvuru belgesi bağlantıları.

## <a name="next-steps"></a>Sonraki Adımlar
[Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma](key-vault-network-security.md)

Bir yöneticiye yönelik başlama öğreticisi için bkz. [Azure anahtar kasası ile çalışmaya başlama](key-vault-get-started.md).

Anahtar Kasası'na yönelik kullanım günlüğü hakkında daha fazla bilgi için bkz. [Azure anahtar kasası günlüğü](key-vault-logging.md).

Azure anahtar kasası ile anahtarları ve gizli anahtarları kullanma hakkında daha fazla bilgi için bkz. [Anahtarlar ve Gizli Anahtarlar Hakkında](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Anahtar kasası ile ilgili sorularınız varsa bkz. [Azure Anahtar Kasası Forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)

