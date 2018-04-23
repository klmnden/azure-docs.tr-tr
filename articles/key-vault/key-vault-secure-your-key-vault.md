---
title: Anahtar kasanızın güvenliğini sağlama | Microsoft Belgeleri
description: Kasaları ve anahtarlar ile parolaları yönetmek için anahtar kasasına yönelik erişim izinlerini yönetin. Anahtar kasasına yönelik kimlik doğrulama ve yetkilendirme modeli ve anahtar kasanızın güvenliğini sağlama
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
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
ms.openlocfilehash: 3a769d15fe79a56d623399d0d38b6dd9c060db36
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="secure-your-key-vault"></a>Anahtar kasanızın güvenliğini sağlama
Azure Anahtar Kasası, bulut uygulamalarınıza ait şifreleme anahtarlarını ve parolaları (sertifikalar, bağlantı dizeleri, parolalar gibi) koruyan bir bulut hizmetidir. Bu veriler hassas ve iş için önemli olduğundan, anahtar kasalarınıza erişimi yalnızca yetkili uygulamaların ve kullanıcıların erişebileceği şekilde güvenli hale getirmek istersiniz. Bu makalede anahtar kasası erişim modeline genel bakış sunulmakta, kimlik doğrulaması ve yetkilendirme açıklanmakta ve bulut uygulamalarınız için anahtar kasasına erişimin güvenliğini sağlama işlemi bir örnekle anlatılmaktadır.

## <a name="overview"></a>Genel Bakış
Bir anahtar kasasına erişim, iki ayrı arabirim ile denetlenir: yönetim düzlemi ve veri düzlemi. Her iki düzlem için de bir çağıranın (kullanıcı ya da uygulama) anahtar kasasına erişim elde edebilmesi için uygun kimlik doğrulaması ve yetkilendirme gereklidir. Kimlik doğrulama işlemi çağıranın kimliğini, yetkilendirme işlemi ise çağıranın hangi işlemleri yapmaya izinli olduğunu belirler.

Kimlik doğrulaması için yönetim düzlemi ve veri düzleminde Azure Active Directory kullanılır. Yetkilendirme için yönetim düzleminde rol tabanlı erişim denetimi (RBAC), veri düzleminde ise anahtar kasası erişim ilkesi kullanılır.

Ele alınan konulara kısa bir genel bakış aşağıda verilmiştir:

[Azure Active Directory kullanarak kimlik doğrulaması](#authentication-using-azure-active-directory) - Bu bölümde bir çağıranın anahtar kasasına yönetim düzlemi ve veri düzlemi üzerinden erişmek için Azure Active Directory ile kimlik doğrulaması yapma işlemi açıklanmaktadır. 

[Yönetim düzlemi ve veri düzlemi](#management-plane-and-data-plane) - Yönetim düzlemi ve veri düzlemi, anahtar kasanıza erişmek için kullanılan iki erişim düzlemidir. Her erişim düzlemi belirli işlemleri destekler. Bu bölümde erişim uç noktaları, desteklenen işlemler ve her bir düzlem tarafından kullanılan erişim denetimi açıklanmaktadır. 

[Yönetim düzlemi erişim denetimi](#management-plane-access-control) - Bu bölümde rol tabanlı erişim denetimi kullanılarak yönetim düzlemi işlemlerine erişim izni verme konusu ele alınacaktır.

[Veri düzlemi erişim denetimi](#data-plane-access-control) - Bu bölümde veri düzlemi erişimini denetlemek için anahtar kasası erişim ilkesini kullanma işlemi açıklanmaktadır.

[Örnek](#example) - Bu örnekte üç farklı ekibin (güvenlik ekibi, geliştiriciler/operatörler ve denetçiler) Azure’da bir uygulama geliştirmek, yönetmek ve izlemek üzere belirli görevleri gerçekleştirmesine olanak tanımak amacıyla anahtar kasanıza yönelik erişim denetiminin nasıl ayarlanacağı açıklanmaktadır.

## <a name="authentication-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak kimlik doğrulama
Bir Azure aboneliğinde yeni bir anahtar kasası oluşturduğunuzda, kasa bu aboneliğin Azure Active Directory kiracısı ile otomatik olarak ilişkilendirilir. Tüm çağıranların (kullanıcılar ve uygulamalar) bu anahtar kasasına erişmek için bu kiracıya kaydedilmesi gerekir. Bir uygulama veya kullanıcının anahtar kasasına erişebilmesi için Azure Active Directory kimlik doğrulaması yapması gerekir. Bu durum hem yönetim düzlemi hem de veri düzlemi erişimi için geçerlidir. Her iki durumda da bir uygulama, anahtar kasasına iki şekilde erişebilir:

* **kullanıcı+uygulama erişimi** - bu seçenek genellikle oturum açmış bir kullanıcı adına anahtar kasasına erişen uygulamalar için geçerlidir. Azure PowerShell ve Azure Portalı bu tür erişimin örneğidir. Kullanıcılara iki yolla erişim verilebilir: bir yolu, anahtar kasasına herhangi bir uygulamadan erişebilmeleri için kullanıcılara erişim verilmesi, diğer yolu ise kullanıcıya yalnızca belirli bir uygulamayı kullandıklarında (birleşik kimlik olarak adlandırılır) anahtar kasası erişimi verilmesidir. 
* **yalnızca uygulama erişimi** - daemon hizmetleri, arka plan işleri, vb. çalıştıran uygulamalar içindir. Uygulamanın kimliğine anahtar kasası erişimi verilir.

Her iki uygulama türünde de, [desteklenen kimlik doğrulama yöntemlerinden](../active-directory/active-directory-authentication-scenarios.md) biri kullanılarak uygulamanın Azure Active Directory kimlik doğrulaması yapılır ve uygulama bir belirteç alır. Kullanılan kimlik doğrulama yöntemi, uygulama türüne bağlıdır. Uygulama bundan sonra bu belirteci kullanıcı ve anahtar kasasına REST API isteği gönderir. Yönetim düzlemi erişiminde istekler Azure Resource Manager uç noktasından yönlendirilir. Veri düzlemine erişirken uygulamalar bir anahtar kasası uç noktasıyla doğrudan konuşur. [Kimlik doğrulama akışının tamamı](../active-directory/active-directory-protocols-oauth-code.md) hakkında daha fazla bilgi edinin. 

Uygulamanın belirteç istediği kaynak adı, uygulamanın yönetim düzlemine veya veri düzlemine erişmesine bağlı olarak değişir. Bu nedenle kaynak adı, Azure ortamına bağlı olarak sonraki bir bölümde bulunan tablodaki yönetim düzlemi veya veri düzlemi uç noktasıdır.

Hem yönetim hem de veri düzleminde kimlik doğrulaması için tek bir mekanizmaya sahip olunması kendi avantajlarına sahiptir:

* Kuruluşlar, kuruluş içindeki tüm anahtar kasalarına erişimi merkezi olarak denetleyebilir
* Bir kullanıcı ayrılırsa, kuruluştaki tüm anahtar kasalarına erişimini kaybeder
* Kuruluşlar Azure Active Directory’deki seçenekleri kullanarak kimlik doğrulamasını özelleştirebilir (örneğin, daha fazla güvenlik için çok faktörlü kimlik doğrulamasını etkinleştirme)

## <a name="management-plane-and-data-plane"></a>Yönetim düzlemi ve veri düzlemi
Azure Anahtar Kasası, Azure Resource Manager dağıtım modeli üzerinden kullanılabilen bir Azure hizmetidir. Bir anahtar kasası oluşturduğunuzda, içinde anahtar, parola ve sertifika gibi diğer nesneleri oluşturabileceğiniz sanal bir kapsayıcı elde edersiniz. Sonra belirli işlemleri gerçekleştirmek üzere yönetim düzlemi ve veri düzlemini kullanarak anahtar kasanıza erişirsiniz. Yönetim düzlemi arabirimi; anahtar kasası özniteliklerini oluşturma,i silme, güncelleştirme ve veri düzlemi için erişim ilkelerini ayarlama gibi işlemlerle anahtar kasanızı yönetmek için kullanılır. Veri düzlemi arabirimi, anahtar kasanıza depolanmış anahtar, parola ve sertifikaları eklemek, silmek, değiştirmek ve kullanmak için kullanılır.

Yönetim düzlemi ve veri düzlemi arabirimlerine farklı uç noktalardan erişilir (tabloya bakın). Tablodaki ikinci sütun, farklı Azure ortamlarında bu uç noktaların DNS adlarını açıklamaktadır. Üçüncü sütunda, her erişim düzleminde gerçekleştirebileceğiniz işlemler açıklanır. Her erişim düzlemi kendi erişim denetimi mekanizmasına sahiptir: yönetim düzlemi için erişim denetimi Azure Resource Manager Rol Tabanlı Access Control (RBAC) kullanılarak ayarlanırken veri düzlemi erişim denetimi, anahtar kasası erişim ilkesi kullanılarak ayarlanır.

| Erişim düzlemi | Erişim uç noktaları | İşlemler | Erişim denetimi mekanizması |
| --- | --- | --- | --- |
| Yönetim düzlemi |**Genel:**<br> management.azure.com:443<br><br> **Azure Çin:**<br> management.chinacloudapi.cn:443<br><br> **Azure ABD:**<br> management.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> management.microsoftazure.de:443 |Anahtar kasası oluşturma/okuma/güncelleştirme/silme <br> Anahtar kasası için erişim ilkelerini ayarlama<br>Anahtar kasası etiketlerini ayarlama |Azure Resource Manager Rol Tabanlı Access Control (RBAC) |
| Veri düzlemi |**Genel:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Azure Çin:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure ABD:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |Anahtarlar için: Şifre Çöz, Şifrele, UnwrapKey, WrapKey, Doğrula, İmzala, Al, Listele, Güncelleştir, Oluştur, İçeri Aktar, Sil, Yedekle, Geri Yükle<br><br> Parolalar için: Al, Listele, Ayarla, Sil |Anahtar kasası erişim ilkesi |

Yönetim düzlemi ve veri düzlemi erişim denetimleri birbirinden bağımsız olarak çalışır. Örneğin, bir uygulamaya anahtar kasasındaki anahtarları kullanmak üzere erişim vermek istiyorsanız, yalnızca anahtar kasası erişim ilkelerini kullanarak veri düzlemi erişim izinleri vermeniz gerekir ve bu uygulama için bir yönetim düzlemi erişimi gerekli değildir. Buna karşılık, bir kullanıcının kasa özelliklerini ve etiketlerini okuyabilmesini, ancak anahtar, parola veya sertifikalara erişmemesini istiyorsanız, RBAC kullanarak bu kullanıcıya 'okuma' erişimi verebilirsiniz ve veri düzlemine erişim gerekmez.

## <a name="management-plane-access-control"></a>Yönetim düzlemi erişim denetimi
Yönetim düzlemi, anahtar kasasını etkileyen işlemlerden oluşur. Örneğin, bir anahtar kasası oluşturabilir veya silebilirsiniz. Bir abonelikteki kasaların listesini alabilirsiniz. Anahtar kasası özelliklerini alabilir (SKU, etiketler gibi) ve kasadaki anahtarlara ve parolalara erişebilen kullanıcıları ve uygulamaları denetleyen anahtar kasası erişim ilkelerini ayarlayabilirsiniz. Yönetim düzlemi erişim denetimi, RBAC kullanır. Önceki bölümde yer alan tabloda, yönetim düzlemi üzerinden gerçekleştirilebilecek anahtar kasası işlemlerinin tam listesini görebilirsiniz. 

### <a name="role-based-access-control-rbac"></a>Rol Tabanlı Access Control (RBAC)
Her Azure aboneliği bir Azure Active Directory’ye sahiptir. Bu dizindeki kullanıcılara, gruplara ve uygulamalara, Azure aboneliğinde Azure Resource Manager dağıtım modelini kullanan kaynakları yönetmek üzere erişim verilebilir. Bu erişim denetimi türü, Rol Tabanlı Access Control (RBAC) olarak adlandırılır. Bu erişimi yönetmek için [Azure portalı](https://portal.azure.com/), [Azure CLI araçları](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) veya [Azure Resource Manager REST API'lerini](https://msdn.microsoft.com/library/azure/dn906885.aspx) kullanabilirsiniz.

Azure Resource Manager modeliyle, bir kaynak grubunda anahtar kasanızı oluşturur ve Azure Active Directory’yi kullanarak bu anahtar kasasının yönetim düzlemine erişimi denetlersiniz. Örneğin, kullanıcılara veya bir gruba, belirli bir kaynak grubunun içindeki anahtar kasalarını yönetme olanağı verebilirsiniz.

Belirli bir kapsamdaki kullanıcılara, gruplara ve uygulamalara uygun RBAC rollerini atayarak erişim verebilirsiniz. Örneğin, bir kullanıcıya anahtar kasalarını yönetmesi için erişim vermek amacıyla, belirli bir kapsamda bu kullanıcıya önceden tanımlı bir 'anahtar kasasına Katkıda Bulunan' rolü atarsınız. Bu örnekteki kapsam bir abonelik, bir kaynak grubu veya yalnızca belirli bir anahtar kasası olabilir. Abonelik düzeyinde atanmış bir rol, bu abonelikteki tüm kaynak grupları ve kaynaklar için geçerlidir. Kaynak grubu düzeyinde atanmış bir rol, ilgili kaynak grubundaki tüm kaynaklar için geçerli olur. Belirli bir kaynağa atanmış bir rol, yalnızca ilgili kaynak için geçerli olur. Önceden tanımlı birkaç rol vardır (bkz. [RBAC: Yerleşik roller](../role-based-access-control/built-in-roles.md)) ve önceden tanımlı roller ihtiyaçlarınıza uygun değilse kendi rollerinizi de tanımlayabilirsiniz.

> [!IMPORTANT]
> Bir kullanıcı bir anahtar kasası yönetim düzleminde Katkıda Bulunan izinlerine (RBAC) sahipse, veri düzlemine erişimi denetleyen anahtar kasası erişim ilkesini ayarlayarak, kendisine veri düzlemine erişim verebilir. Bu nedenle, anahtar kasalarınıza, anahtarlarınıza, parolalarınıza ve sertifikalarınıza erişip bunları yönetebilen kullanıcıların yalnızca yetkili kişiler olduğundan emin olmak amacıyla anahtar kasalarınıza 'Katkıda Bulunan' erişimine kimlerin sahip olduğunun sıkı denetlenmesi önerilir.
> 
> 

## <a name="data-plane-access-control"></a>Veri düzlemi erişim denetimi
Anahtar kasası veri düzlemi, bir anahtar kasasındaki anahtarlar, parolalar ve sertifikalar gibi nesneleri etkileyen işlemlerden oluşur.  Buna anahtar oluşturma, içeri aktarma, güncelleştirme, listeleme, yedekleme ve geri yükleme gibi anahtar işlemleri, anahtar etiketlerini ve diğer özniteliklerini imzalama, doğrulama, şifreleme, şifresini çözme, sarmalama, sarmalamadan çıkarma ve ayarlama gibi şifreleme işlemleri dahildir. Benzer şekilde, parolalar için alma, ayarlama, listeleme ve silme işlemleri dahildir.

Veri düzlemi erişimi, bir anahtar kasasına ilişkin erişim ilkeleri ayarlanarak verilir. Bir kullanıcı, grup veya uygulamanın bir anahtar kasasına ait erişim ilkelerini ayarlayabilmesi için, ilgili anahtar kasasının yönetim düzleminde Katkıda Bulunan izinlerine (RBAC) sahip olması gerekir. Bir kullanıcı, grup veya uygulamaya, bir anahtar kasasındaki anahtarlar veya parolalar için belirli işlemleri gerçekleştirmek üzere erişim verilebilir. Key Vault, bir anahtar kasası için en fazla 16 adet erişim ilkesi girişi destekler. Bir Azure Active Directory güvenlik grubu oluşturun ve bir anahtar kasasındaki birkaç kullanıcıya veri düzlemi erişimi vermek amacıyla ilgili gruba kullanıcılar ekleyin.

### <a name="key-vault-access-policies"></a>Key Vault Erişim İlkeleri
Key Vault erişim ilkeleri, anahtarlara, parolalara ve sertifikalara ayrı ayrı izinler verir. Örneğin, bir kullanıcıya parolalar için herhangi bir izin vermeden yalnızca anahtarlar için erişim verebilirsiniz. Ancak, anahtar, parola veya sertifikalara erişim izni kasa düzeyinde verilir. Diğer bir deyişle, anahtar kasası erişim ilkesi nesne düzeyinde izinleri desteklemez. Bir anahtar kasasının erişim ilkelerini ayarlamak için [Azure portalı](https://portal.azure.com/), [Azure CLI araçları](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) veya [anahtar kasası Yönetim REST API’lerini](https://msdn.microsoft.com/library/azure/mt620024.aspx) kullanabilirsiniz.

> [!IMPORTANT]
> Anahtar kasası erişim ilkelerinin kasa düzeyinde geçerli olduğunu unutmayın. Örneğin, bir kullanıcıya anahtar oluşturma ve silme izni verildiğinde kullanıcı bu işlemleri ilgili anahtar kasasındaki tüm anahtarlar üzerinde gerçekleştirebilir.
> 
> 

## <a name="example"></a>Örnek
SSL için sertifika, verileri depolamak için Azure depolama ve imzalama işlemleri için RSA 2048 bit anahtarı kullanan bir uygulama geliştirdiğinizi varsayalım. Bu uygulamanın bir VM (veya VM Ölçek Kümesi) içinde çalıştığını kabul edelim. Tüm uygulama parolalarını depolamak için bir anahtar kasa kullanabilir ve anahtar kasasını kullanarak, uygulamanın Azure Active Directory kimlik doğrulaması için kullandığı önyükleme sertifikasını depolayabilirsiniz.

Bu nedenle, bir anahtar kasasına depolanacak tüm anahtar ve parolaların özeti aşağıda verilmiştir.

* **SSL Sertifikası** - SSL için kullanılır
* **Depolama Anahtarı** - Depolama hesabına erişmek için kullanılır
* **RSA 2048 bit anahtarı** - imzalama işlemleri için kullanılır
* **Önyükleme sertifikası** - Azure Active Directory kimlik doğrulaması yapmak, depolama anahtarını getirmek üzere anahtar kasasına erişim elde etmek ve imzalama için RSA anahtarını kullanmak üzere kullanılır.

Şimdi de bu uygulamayı yöneten, dağıtan ve denetleyen kişilerle tanışalım. Bu örnekte üç rol kullanılacaktır.

* **Güvenlik ekibi** - Bunlar genellikle 'CSO (Güvenlik Müdürü) ofisi' veya eşdeğerindeki bir bölümde, SSL sertifikaları, imzalama için kullanılan RSA anahtarları, veritabanlarına ait bağlantı dizeleri ve depolama hesabı anahtarlarının doğru korunmasından sorumlu BT personelidir.
* **Geliştiriciler/operatörler** - Bunlar bu uygulamayı geliştirip Azure’da dağıtan kişilerdir. Genellikle, güvenlik ekibinin parçası değildir ve bu nedenle SSL sertifikaları ve RSA anahtarları gibi hassas verilerin erişimine sahip olmaması gerekir, ancak dağıttıkları uygulama bu erişime sahip olmalıdır.
* **Denetçiler** - Bunlar genellikle geliştiricilerden ve genel BT personelinden ayrı tutulan farklı bir kullanıcı grubudur. Sorumlulukları arasında sertifika, anahtar, vb. doğru kullanımını ve bakımını incelemek ve veri güvenliği standartlarına uygunluğu sağlamak bulunur. 

Bu uygulamanın kapsamı dışında olup burada bahsedilecek bir rol daha mevcuttur: abonelik (veya kaynak grubu) yöneticisi. Abonelik yöneticisi, güvenlik ekibi için ilk erişim izinlerini ayarlar. Burada, abonelik yöneticisinin bu uygulama için gerekli olan tüm kaynakların bulunduğu bir kaynak grubu için güvenlik ekibine erişim verdiği kabul edilmektedir.

Her bir rolün bu uygulama bağlamında gerçekleştirdiği işlemler aşağıda verilmiştir.

* **Güvenlik ekibi**
  * Anahtar kasaları oluşturma
  * Anahtar kasası günlüğünü açma
  * Anahtar/parola ekleme
  * Olağanüstü durum kurtarma için anahtarların yedeğini oluşturma
  * Kullanıcılara ve uygulamalara belirli işlemleri gerçekleştirmesi için izinler veren anahtar kasası erişim ilkesini ayarlama
  * Anahtarları/parolaları düzenli aralıklarla alma
* **Geliştiriciler/operatörler**
  * Güvenlik ekibinden önyükleme ve SSL sertifikalarının (parmak izleri), depolama anahtarının (güvenlik URI’si) ve imzalama anahtarının (Anahtar URI’si) başvurularını alma
  * Anahtarlara ve parolalara programlı olarak erişen uygulamayı geliştirme ve dağıtma
* **Denetçiler**
  * Uygun anahtar/parola kullanımını ve veri güvenliği standartlarına uyumu onaylamak üzere kullanım günlüklerini gözden geçirme

Her bir rolün (ve uygulamanın) atanmış görevleri gerçekleştirmesi için gereken anahtar kasası erişim izinlerinin ne olduğu aşağıda açıklanmaktadır. 

| Kullanıcı Rolü | Yönetim düzlemi izinleri | Veri düzlemi izinleri |
| --- | --- | --- |
| Güvenlik Ekibi |anahtar kasasına Katkıda Bulunan |Anahtarlar: yedekleme, oluşturma, silme, alma, içeri aktarma, listeleme, geri yükleme <br> Parolalar: tümü |
| Geliştiriciler/Operatörler |dağıttıkları VM’lerin parolaları anahtar kasasından getirebilmesi anahtar kasası dağıtma izni |None |
| Denetçiler |Yok |Anahtarlar: listeleme<br>Parolalar: listeleme |
| Uygulama |Yok |Anahtarlar: imzalama<br>Parolalar: imzalama |

> [!NOTE]
> Denetçilerin etiketler, etkinleştirme ve sona erme tarihleri gibi günlüklerde verilmeyen anahtar ve parola özniteliklerini inceleyebilmesi için anahtar ve parolalara yönelik listeleme izni gereklidir.
> 
> 

Anahtar kasası iznine ek olarak üç rolün tamamı da diğer kaynaklara erişebilmelidir. Örneğin, VM’leri (veya Web Apps, vb.) dağıtabilmek için Geliştiriciler/Operatörler aynı zamanda bu kaynak türlerine 'Katkıda Bulunan' erişimine sahip olmalıdır. Denetçiler, anahtar kasası günlüklerinin depolandığı depolama hesabına okuma erişimine sahip olmalıdır.

Bu makalenin odak noktası anahtar kasanıza erişimin güvenliğinin sağlanması olduğundan, yalnızca bu konuyla ilgili kısımları açıklayacak ve sertifika dağıtımı, anahtar ve parolalara programlı erişim, vb. ayrıntıları atlayacağız. Bu ayrıntıları başka bölümlerde zaten ele alınmaktadır. Anahtar kasasına depolanmış sertifikaların VM’lere dağıtılması bir [blog gönderisinde](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) ele alınmıştır ve önyükleme sertifikası kullanarak anahtar kasası erişimi elde etmek üzere Azure AD kimlik doğrulaması yapma işlemini gösteren bir [örnek kod](https://www.microsoft.com/download/details.aspx?id=45343) mevcuttur.

Erişim izinlerinin büyük bölümü Azure portalı kullanılarak verilebilir, ancak istediğiniz sonucu elde etmek üzere ayrıntılı izinler vermek için Azure PowerShell (veya Azure CLI) kullanmanız gerekir. 

Aşağıdaki PowerShell kod parçacıkları şu varsayımlara sahiptir:

* Azure Active Directory yöneticisi Contoso Güvenlik Ekibi, Contoso Uygulama Devops, Contoso Uygulama Denetçileri adlı üç rolü temsil eden güvenlik grupları oluşturmuştur. Yönetici ayrıca ait oldukları gruplara kullanıcılar eklemiştir.
* **ContosoAppRG** tüm kaynakların bulunduğu kaynak grubudur. **contosologstorage**, günlüklerin depolandığı yerdir. 
* **ContosoKeyVault** anahtar kasası ve **contosologstorage** anahtar kasası günlükleri için kullanılan depolama hesabı aynı Azure konumunda olmalıdır

İlk olarak, abonelik yöneticisi güvenlik ekibine 'anahtar kasasına Katkıda Bulunan' ve 'Kullanıcı Erişimi Yöneticisi' rollerini atar. Bunun yapılması, güvenlik ekibinin diğer kaynaklara erişmesine ve diğer kaynaklara erişim ile ContosoAppRG kaynak grubundaki anahtar kasalarını yönetmesine olanak tanır.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Aşağıdaki komut dosyasında güvenlik ekibinin anahtar kasası oluşturması, günlük oluşturması ve diğer rol ve uygulamalara ilişkin erişim izinlerini ayarlaması gösterilmektedir. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets all

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

Tanımlanan özel rol yalnızca ContosoAppRG kaynak grubunun oluşturulduğu aboneliğe atanabilir. Diğer aboneliklerdeki diğer projeler için de aynı özel roller kullanılacaksa, kapsamına daha fazla abonelik eklenebilir.

Geliştiriciler/operatörler için "dağıtma/işlem" iznine yönelik özel rol ataması, kaynak grubu kapsamında yapılır. Bu şekilde yalnızca 'ContosoAppRG' kaynak grubunda oluşturulan VM’ler parolaları (SSL sertifikası ve önyükleme sertifikası) alır. Dev/ops ekibinin bir üyesi tarafından diğer kaynak grubunda oluşturulan VM’ler, parola URI’lerini bilseler bile bu parolaları alamaz.

Bu örnekte basit bir senaryo gösterilmektedir. Gerçek yaşam senaryoları daha karmaşık olabilir ve gereksinimlerinize göre izinleri anahtar kasanıza uyarlamanız gerekebilir. Örneğin, bu örnekte güvenlik ekibinin uygulamalarında başvurmaları gereken anahtar ve parola başvurularını (URI’ler ve parmak izleri) sağladığı varsayılmaktadır. Bu nedenle, geliştiricilere/operatörlere herhangi bir veri düzlemi erişimi vermez. Ayrıca, bu örnekte anahtar kasanızın güvenliğini sağlama konusuna odaklanılmıştır. [VM’leriniz](https://azure.microsoft.com/services/virtual-machines/security/), [depolama hesaplarınız](../storage/common/storage-security-guide.md) ve diğer Azure kaynaklarının güvenliğini sağlamak için de benzer önem verilmelidir.

> [!NOTE]
> Not: Bu örnekte, üretim sırasında anahtar kasası erişiminin nasıl kilitleneceği gösterilmektedir. Geliştiriciler uygulamayı geliştirdikleri kasalar, VM’ler ve depolama hesabını yönetmek için tam izinlerinin olduğu aboneliklere veya kaynak grubuna sahip olmalıdır.
> 
> 

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
  
  Bu bağlantı 2015 MS Ignite konferansının 9. Kanalındaki videoya aittir. Bu oturumda, Azure’daki erişim yönetimi ve raporlama özellikleri konuşulmakta ve Azure Active Directory'yi kullanarak Azure aboneliklerine erişimin güvenliğini sağlama konusundaki en iyi uygulamalar keşfedilmektedir.
* [OAuth 2.0 ve Azure Active Directory kullanarak web uygulamalarına erişim yetkisi verme](../active-directory/active-directory-protocols-oauth-code.md)
  
  Bu makalede Azure Active Directory ile kimlik doğrulaması için tam OAuth 2.0 akışı açıklanmaktadır.
* [anahtar kasası Yönetim REST API’leri](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Bu belge, anahtar kasası erişim ilkesinin ayarlanması dahil olmak üzere anahtar kasanızı programlı olarak yönetmeye yönelik REST API’leri başvurusudur.
* [anahtar kasası REST API’leri](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  Anahtar kasası REST API başvuru belgelerinin bağlantısı.
* [Anahtar erişim denetimi](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  Gizli anahtar erişim denetimi başvuru belgelerinin bağlantısı.
* [Gizli anahtar erişim denetimi](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  Anahtar erişim denetimi başvuru belgelerinin bağlantısı.
* PowerShell kullanarak anahtar kasası erişimini [ayarlama](https://msdn.microsoft.com/library/mt603625.aspx) ve [kaldırma](https://msdn.microsoft.com/library/mt619427.aspx)
  
  Anahtar kasası erişim ilkesini yönetmeye yönelik PowerShell cmdlet’lerinin başvuru belgesi bağlantıları.

## <a name="next-steps"></a>Sonraki Adımlar
Bir yöneticiye yönelik başlama öğreticisi için bkz. [Azure anahtar kasası ile çalışmaya başlama](key-vault-get-started.md).

Anahtar Kasası'na yönelik kullanım günlüğü hakkında daha fazla bilgi için bkz. [Azure anahtar kasası günlüğü](key-vault-logging.md).

Azure anahtar kasası ile anahtarları ve gizli anahtarları kullanma hakkında daha fazla bilgi için bkz. [Anahtarlar ve Gizli Anahtarlar Hakkında](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Anahtar kasası ile ilgili sorularınız varsa bkz. [Azure Anahtar Kasası Forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)

